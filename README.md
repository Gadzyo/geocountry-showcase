<div align="center">

# 🌍 GeoCountry

### Bot Discord de roleplay géopolitique — économie, diplomatie & opérations militaires
### *Geopolitical roleplay Discord bot — economy, diplomacy & military operations*

**Python 3.12 · discord.py · SQLite · Déployé en production / Live in production**

[🇫🇷 Français](#-français) · [🇬🇧 English](#-english) · [💼 Me contacter / Hire me](#-travaillons-ensemble--lets-work-together)

</div>

---

<!-- Remplace ce bloc par une capture d'ensemble (cockpit principal ou panneau économie). -->
<!-- Replace this block with a hero screenshot (main cockpit or economy panel). -->
> 📸<img width="963" height="452" alt="image" src="https://github.com/user-attachments/assets/feaff224-420a-4ec9-bf15-15b49beb0eef" />

*

---

## 🇫🇷 Français

**GeoCountry** est un bot Discord complet qui fait tourner un serveur de roleplay géopolitique : des dizaines de nations gérées par de vrais joueurs, avec une économie réelle, un système de gouvernement, des opérations militaires et de la diplomatie. Le bot est **déployé en production** et utilisé quotidiennement.

> Ce projet démontre ma capacité à concevoir, développer, déployer et maintenir un bot Discord complexe de bout en bout — base de données, logique métier, sécurité et expérience utilisateur soignée.

### ✨ Ce que fait le bot

| Système | Description |
|---|---|
| 💰 **Économie complète** | Budgets nationaux, transactions entre pays, dépenses d'État, relevés économiques automatiques par nation. Tout est calculé et persisté en base de données. |
| 🔑 **Gouvernement & délégation** | Permissions multi-niveaux : Chef d'État, gouvernement, délégation de pouvoirs (transferts, votes, opérations militaires) à plusieurs membres, avec notifications privées. |
| 🪖 **Opérations militaires** | Workflow complet de planification d'opérations avec validation par le staff, opérations secrètes (espionnage), et machine à états (brouillon → soumission → validation). |
| 🏛️ **Conseil de Sécurité (ONU)** | Système de vote, mandats, sièges permanents et non-permanents — une vraie mécanique diplomatique. |
| 🎛️ **Interfaces interactives** | Panneaux à boutons (« cockpits »), menus déroulants, modals de saisie — l'UX moderne de Discord, pas de vieilles commandes texte. |
| 🔒 **Sécurité & anti-raid** | Moteur de détection gradué, système de lockdown, protection contre les attaques — pensé pour un serveur réel. |
| 📅 **Calendrier RP** | Gestion automatique du temps de jeu, dates contextualisées, notifications ciblées. |

### 🛠️ Sous le capot (technique)

- **Architecture propre** : code modulaire (cogs), séparation des responsabilités, conventions documentées.
- **Base de données** : SQLite / aiosqlite avec transactions atomiques, gestion de la concurrence (anti-collision sur les écritures critiques).
- **Sécurité** : système de permissions centralisé, gardes vérifiées à chaque action sensible, audit des opérations.
- **Qualité** : historique Git soigné (un commit = une intention), revue systématique, déploiement contrôlé sur VPS (systemd).
- **Maintenance** : le bot évolue en continu sans interruption de service pour les joueurs.

---

## 🇬🇧 English

**GeoCountry** is a full-featured Discord bot powering a geopolitical roleplay server: dozens of nations run by real players, with a real economy, a government system, military operations and diplomacy. The bot is **deployed in production** and used daily.

> This project demonstrates my ability to design, build, deploy and maintain a complex Discord bot end-to-end — database, business logic, security and polished user experience.

### ✨ What the bot does

| System | Description |
|---|---|
| 💰 **Full economy** | National budgets, country-to-country transactions, state spending, automatic per-nation economic statements. Everything is computed and persisted in a database. |
| 🔑 **Government & delegation** | Multi-level permissions: Head of State, government, delegation of powers (transfers, votes, military operations) to multiple members, with private notifications. |
| 🪖 **Military operations** | Complete operation-planning workflow with staff validation, secret (espionage) operations, and a state machine (draft → submission → approval). |
| 🏛️ **Security Council (UN)** | Voting system, mandates, permanent and non-permanent seats — a real diplomatic mechanic. |
| 🎛️ **Interactive interfaces** | Button panels ("cockpits"), dropdown menus, input modals — modern Discord UX, not legacy text commands. |
| 🔒 **Security & anti-raid** | Graduated detection engine, lockdown system, attack protection — built for a real server. |
| 📅 **RP calendar** | Automatic in-game time management, contextual dates, targeted notifications. |

### 🛠️ Under the hood (technical)

- **Clean architecture**: modular code (cogs), separation of concerns, documented conventions.
- **Database**: SQLite / aiosqlite with atomic transactions and concurrency handling (collision-safe critical writes).
- **Security**: centralized permission system, guards checked on every sensitive action, operation auditing.
- **Quality**: careful Git history (one commit = one intent), systematic review, controlled VPS deployment (systemd).
- **Maintenance**: the bot evolves continuously with zero downtime for players.

---

## 📸 Galerie / Gallery

<!-- Ajoute tes captures ici. Une ligne par fonctionnalité, avec une légende. -->
<!-- Add your screenshots here. One row per feature, with a caption. -->
<img width="865" height="581" alt="image" src="https://github.com/user-attachments/assets/8b98fd5d-0405-4f08-bba5-5a50761a6d27" />
<img width="922" height="369" alt="image" src="https://github.com/user-attachments/assets/cc44f976-6509-4ad3-8c40-4882142b7c59" />
<img width="1008" height="882" alt="image" src="https://github.com/user-attachments/assets/0f06edae-7ba7-407c-b626-fe94eac63ca2" />


| | |
|---|---|
| 💰 *Système économique / Economy* | 🔑 *Délégation / Delegation* |
| *[capture]* | *[capture]* |
| 🪖 *Opérations militaires / Military ops* | 🎛️ *Cockpit / Panels* |
| *[capture]* | *[capture]* |

---

## 💼 Travaillons ensemble / Let's work together

🇫🇷 **Je développe des bots Discord sur-mesure** — du bot de modération simple au système complet avec base de données (économie, niveaux, tickets, panneaux interactifs). Si vous avez un projet, parlons-en.

🇬🇧 **I build custom Discord bots** — from simple moderation bots to full database-driven systems (economy, leveling, tickets, interactive panels). Got a project? Let's talk.

| | |
|---|---|
| 📦 **Pack Essentiel / Essential** | Modération, bienvenue, rôles, commandes custom / *Moderation, welcome, roles, custom commands* |
| 📦 **Pack Avancé / Advanced** ⭐ | Base de données, économie, panneaux, logique sur-mesure / *Database, economy, panels, custom logic* |
| 📦 **Pack Sur-mesure / Custom** | Projet complet type GeoCountry / *Full project like GeoCountry* |
| 🔧 **Maintenance** | Support & mises à jour mensuels / *Monthly support & updates* |

### 📬 Contact
<!-- Remplace par tes vrais liens. / Replace with your real links. -->
- **Discord** : `ton_pseudo`
- **GitHub** : [Gadzyo](https://github.com/Gadzyo)
- *(Fiverr / email / autre)*

---

<div align="center">

*Built with Python 🐍 · discord.py · SQLite · Beaucoup de café ☕*

</div>
