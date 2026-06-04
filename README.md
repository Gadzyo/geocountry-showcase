# 🌐 GeoCountry

Bot Discord de **roleplay géopolitique** en Python (`discord.py`) : une simulation à grande échelle — une centaine de pays actifs, un Conseil de Sécurité de l'ONU, des budgets nationaux, des gouvernements et des processus de vote — entièrement pilotée depuis Discord (commandes slash, panneaux à boutons, forums de candidature).

![version](https://img.shields.io/badge/version-v0.12.0-3bd6c4)
![statut](https://img.shields.io/badge/statut-en%20production-2ea043)
![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![discord.py](https://img.shields.io/badge/discord.py-5865F2?logo=discord&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-systemd-FCC624?logo=linux&logoColor=black)

Il tourne **en production**, 24/7, sur un VPS Linux. Ce n'est pas une démo : de vrais utilisateurs, de vraies données, et les contraintes qui vont avec — disponibilité, cohérence, sécurité.

🔗 Présentation détaillée : **[gadzyo.dev/geocountry](https://gadzyo.dev/geocountry.html)**

> Ce dépôt est une **vitrine technique** : il met en avant quelques extraits représentatifs de l'architecture. Deux pans du projet ne sont volontairement **pas** publiés — la logique **anti-raid** (sécurité du serveur) et les calculs d'**équilibrage économique** — pour des raisons de propriété et de sûreté. Ils sont mentionnés, pas montrés.

---

## 📸 Aperçu

![Tableau de bord économique d'un pays](dashboard.png)

> Budget national généré en direct par le bot : PIB, dépenses obligatoires, solde, statut au Conseil de Sécurité.

## 🛠️ Stack

- **Python 3.12** · `discord.py`
- **SQLite** (accès asynchrone) comme stockage
- Déploiement sur **VPS Linux** (Ubuntu) via un service `systemd`
- Architecture en **cogs** (modules `discord.py`) + une couche de **helpers métier** réutilisables

## 🗂️ Organisation du projet

```
.
├── main.py                  # point d'entrée, chargement des cogs
├── db.py                    # couche d'accès BDD (transactions, helpers async)
├── config.py                # configuration (lue depuis l'environnement)
├── schema.sql               # schéma SQLite (append-only, migrations au boot)
│
├── cogs/                    # modules fonctionnels (un domaine = un cog)
│   ├── budget.py            #   économie nationale
│   ├── cs_onu.py            #   Conseil de Sécurité (votes)
│   ├── cs_session.py        #   sessions du Conseil
│   ├── cs_election.py       #   élections des membres non-permanents
│   ├── fiches_pays.py       #   candidatures « pays » (workflow de validation)
│   ├── fiches_gouverneur.py #   candidatures aux postes de gouvernement
│   ├── cockpit.py           #   panneau de pilotage par pays
│   ├── security.py          #   anti-raid (non détaillé ici)
│   └── …
│
├── utils/                   # helpers métier partagés, testables isolément
│   ├── gouverneurs.py       #   règles de quota des gouvernements
│   ├── pays_lookup.py       #   résolution « membre → pays »
│   ├── permissions.py       #   gardes de rôle / dirigeant
│   └── …
│
├── scripts/                 # migrations & seeds ponctuels
├── docs/                    # décisions d'architecture (ADR)
└── data/                    # bases SQLite locales (non versionnées)
```

Un principe directeur : **une règle métier = une seule source de vérité**. Les règles partagées par plusieurs cogs (quota d'un gouvernement, résolution du pays d'un joueur, calcul de solde) vivent dans `utils/` et sont appelées partout où elles s'appliquent, plutôt que dupliquées.

## 🧩 Extraits commentés

Les exemples ci-dessous tournent autour d'un même fil rouge : **la gestion de la concurrence**. Un bot Discord reçoit des interactions simultanées (deux joueurs qui votent à la même milliseconde, un double-clic sur un bouton, deux commandes lancées coup sur coup). Le code doit produire un résultat correct dans tous ces entrelacements, pas seulement dans le cas idéal.

### 1. La primitive de transaction

Toute opération qui doit être atomique passe par ce context manager asynchrone. Il sérialise les transactions concurrentes derrière un verrou, *commit* en sortie normale et *rollback* sur toute exception — automatiquement.

```python
@asynccontextmanager
async def transaction(self) -> AsyncIterator[Transaction]:
    async with self._tx_lock:
        tx = Transaction(self.conn)
        try:
            yield tx
            await self.conn.commit()
        except Exception:
            await self.conn.rollback()
            raise
```

*C'est la brique réutilisée par tous les extraits suivants : elle garantit qu'un bloc de lectures/écritures s'exécute d'un seul tenant, sans entrelacement avec une autre transaction.*

### 2. Réserver une ressource avant d'agir (compare-and-swap)

Problème : une commande de récapitulatif est protégée par un *cooldown*. Un pré-check en Python ne suffit pas — deux appels rapprochés le passent tous les deux, puis postent deux fois. La solution est de **réserver le créneau** par un `UPDATE` conditionnel : la ligne n'est modifiée que si le cooldown est réellement écoulé, et `changes() == 1` signifie « j'ai gagné le droit de poster ».

```python
async with db.transaction() as tx:
    await tx.execute(
        "UPDATE cs_sessions SET last_recap_at = ? "
        "WHERE id = ? AND (last_recap_at IS NULL OR last_recap_at <= ?)",
        (now_iso, sid, threshold_iso),
    )
    won = (await tx.fetchone("SELECT changes() AS n"))["n"] == 1
if not won:
    # Un autre appel a réservé le créneau entre le pré-check et ici → on ne poste pas.
    ...
```

*Le réflexe clé : réserver la ressource de façon atomique **avant** l'effet de bord coûteux (le post), plutôt que de vérifier puis d'agir en deux temps.*

### 3. Modifier un état partagé sans perte de mise à jour

Problème : plusieurs membres du staff votent sur une même candidature. Chaque vote est fusionné dans un même champ JSON. Si deux votes lisent l'état au même moment, fusionnent chacun leur clé, puis réécrivent — l'un écrase l'autre (*lost update*). La solution est de **relire l'état frais à l'intérieur de la transaction** avant de fusionner, et de re-vérifier au passage que la candidature est toujours ouverte.

```python
async with db.transaction() as tx:
    fresh = await tx.fetchone(
        "SELECT votes_staff FROM fiches_pays "
        "WHERE id = ? AND statut = 'en_vote'",
        (fiche["id"],),
    )
    if fresh is None:
        # La candidature a été résolue entre-temps → on n'écrit pas.
        ...
        return
    try:
        votes = json.loads(fresh["votes_staff"] or "{}")
    except (TypeError, ValueError):
        votes = {}
    votes[str(member.id)] = choice
    await tx.execute(
        "UPDATE fiches_pays SET votes_staff = ? WHERE id = ?",
        (json.dumps(votes), fiche["id"]),
    )
```

*Le réflexe clé : sous le verrou de transaction, on relit la donnée juste avant de la modifier — ce qui garantit que le second votant voit déjà le premier.*

---

## ✅ Ce que ce projet met en pratique

- **Concurrence maîtrisée** — transactions, compare-and-swap, lecture-modification-écriture sérialisée, recours aux contraintes d'intégrité de la base comme garde-fou. Le code est pensé pour les interactions simultanées, pas seulement pour le cas nominal.
- **Sécurité d'accès aux données** — requêtes systématiquement paramétrées.
- **Architecture lisible** — séparation cogs / helpers, règles métier centralisées, fonctions pures testables isolément.
- **Robustesse opérationnelle** — déploiement `systemd`, schéma appliqué au démarrage, gestion explicite des cas dégradés (configuration manquante, ressources Discord absentes) sans planter le bot.

## 👤 Moi

**Gadzyo** — dev, systèmes & sécurité.
🌐 [gadzyo.dev](https://gadzyo.dev) · ✉️ gadzyo@gadzyo.dev

---

*Projet personnel en développement actif. Les sections sensibles (anti-raid, équilibrage économique) ne sont pas incluses dans ce dépôt public.*
