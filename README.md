# 🌐 GeoCountry

> Simulation géopolitique en temps réel sur Discord — économie nationale, Conseil de Sécurité de l'ONU, résolutions. Déployée en continu, **24/7**.

![Version](https://img.shields.io/badge/version-v0.12.0-3bd6c4)
![Statut](https://img.shields.io/badge/statut-en%20production-2ea043)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![discord.py](https://img.shields.io/badge/discord.py-5865F2?logo=discord&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-systemd-FCC624?logo=linux&logoColor=black)

**GeoCountry** est un bot Discord qui fait tourner un jeu de rôle géopolitique : une centaine de pays, pilotés par les joueurs, avec une vraie économie, de la diplomatie et un Conseil de Sécurité de l'ONU. Ce n'est pas un projet-tutoriel — c'est une **application déployée, utilisée et maintenue en production**, avec ses contraintes réelles (disponibilité, cohérence des données, sécurité).

🔗 **Vitrine complète : [gadzyo.dev/geocountry](https://gadzyo.dev/geocountry.html)**

---

## 📸 Aperçu

![Tableau de bord économique d'un pays](docs/dashboard.png)

> Budget national généré en direct par le bot : PIB, dépenses obligatoires, solde, statut au Conseil de Sécurité.

## ✨ Fonctionnalités

- **Économie nationale** — PIB, budgets, dépenses obligatoires et soldes calculés en temps réel.
- **Conseil de Sécurité de l'ONU** — sièges, statuts P5, dépôt et vote de résolutions.
- **Diplomatie & délégations** entre pays.
- **Calendrier de jeu automatique** qui fait avancer le temps RP.
- **Tableau de bord par pays** — embeds et boutons interactifs.
- **Sécurité anti-raid** et gestion fine des permissions.
- **~100 commandes** au total.

## 🏗️ Architecture

Une séparation nette des responsabilités, pour rester maintenable à mesure que le projet grossit :

| Couche | Rôle |
|---|---|
| **Présentation** | Commandes Discord, embeds, boutons interactifs |
| **Service** | Logique de jeu : économie, votes, résolutions, calendrier |
| **Données** | Base **SQLite**, avec migrations de schéma appliquées **en production sans interruption** |

## 🛠️ Stack technique

`Python` · `discord.py` · `SQLite` · `Linux / systemd` · `VPS` · `Git`

## 🧩 Défis de production résolus

- **Concurrence sur les votes simultanés** — bug de concurrence identifié, compris et corrigé (le genre de problème qui n'apparaît que sous charge réelle).
- **Migrations sans coupure** — faire évoluer le schéma de la base alors que le service tourne 24/7, sans perte de données.
- **Sécurité anti-raid** — défense en profondeur, permissions fines.
- **Livraison cadrée** — module « Cockpit » conçu, découpé et livré en une seule session : 10 commits, une intention par commit.

## 📈 État du projet

- Version **v0.12.0** — en production, stable.
- **Roadmap V1** : 8 chantiers planifiés, séquencés du plus proche au plus lointain.

## 🔒 À propos de ce dépôt

Ce dépôt est une **vitrine**. Le code source complet reste **privé**, pour des raisons de **sécurité** (jetons, mécanismes anti-raid) et de **propriété** du jeu. On y présente l'architecture, les fonctionnalités et la démarche — pas l'intégralité du code.

## 👤 Auteur

**Gadzyo** — développeur · systèmes & sécurité.
🌐 [gadzyo.dev](https://gadzyo.dev)  ·  ✉️ gadzyo@gadzyo.dev
