# OpenLink

**Une application de développement IA open source, centrée sur chaque projet.** OpenLink OSS est une application de base que vous pouvez exécuter, étudier et adapter à votre produit ou à votre façon de travailler.

**Langues :** [English](./README.md) · [简体中文](./README.zh-CN.md) · [繁體中文](./README.zh-TW.md) · [日本語](./README.ja-JP.md) · [한국어](./README.ko-KR.md) · Français · [Deutsch](./README.de-DE.md) · [Русский](./README.ru-RU.md)

## Sommaire

- [Démonstration](#démonstration)
- [Fonctionnalités](#fonctionnalités)
- [Démarrage](#démarrage)
- [Arborescence du dépôt](#arborescence-du-dépôt)
- [Personnalisation et mises à jour](#personnalisation-et-mises-à-jour)
- [Évolutions prévues](#évolutions-prévues)
- [Documentation et licence](#documentation-et-licence)

## Démonstration

[![Voir la démonstration OpenLink](./public/openlink/demo/image/encoded-shot-4.png)](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)

[Voir la vidéo complète](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4). La vidéo et les images illustrent le flux de travail ; certaines étapes sont accélérées.

![Conversation et aperçu d'un projet dans OpenLink](./public/openlink/demo/image/encoded-shot-7.png)

## Fonctionnalités

**Chaque projet constitue un espace de travail complet.** Il possède ses fichiers et son historique Git dans `/workspace`, un environnement persistant, des services de développement et un backend applicatif distinct. Les sessions de conversation s'exécutent dans des bacs à sable séparés au sein du projet. Avec le backend VM, chaque projet dispose de sa propre machine Linux et de son propre noyau invité. Le backend de compatibilité par conteneur offre un niveau d'isolation différent, explicitement indiqué.[VM de projet](./docs/architecture/ADR-0007-project-vm-runtime-boundaries.md) · [Classes d'isolation](./docs/architecture/ADR-0022-production-deployment-profiles-and-resource-governance.md)

- **Créer et inspecter :** travaillez avec l'Agent dans une conversation liée au projet, examinez le code dans l'éditeur et visualisez le résultat dans l'aperçu natif ou un navigateur Chromium isolé.[Conception du navigateur](./docs/architecture/ADR-0004-browser-runtime-surfaces.md)
- **Backend par projet :** chaque Project VM peut héberger des services compatibles avec Supabase : Auth, PostgreSQL, Realtime, Storage et Functions. Les données et secrets restent séparés de la base du plan de contrôle.[Cycle de vie](./docs/architecture/standards/04-runtime-lifecycle.md)
- **Architecture multi-locataire :** utilisateurs, organisations, espaces de travail et projets ont des propriétaires explicites. Les services vérifient les droits, et les tables exposées appliquent des politiques de sécurité au niveau des lignes.[Données et sécurité](./docs/architecture/standards/05-data-and-security.md)
- **Local, Cloud et connaissances :** les modes Local et Cloud partagent les mêmes contrats, mais pas leurs identités ni leurs données. Avec un fournisseur d'embeddings configuré, la recherche sémantique applique des filtres par locataire.[Vue du système](./docs/architecture/standards/02-system-context.md)

**Publication des projets :** le déploiement en un clic d'un projet généré vers un serveur choisi ou la machine locale, avec attribution automatique d'un domaine résolu, n'a pas encore été introduit dans l'édition OSS. Il sera ajouté progressivement dans de futures versions OSS. L'auto-hébergement d'OpenLink et l'aperçu des projets constituent des parcours distincts.[Installation auto-hébergée](./docs/operations/self-hosted-installation.md)

## Démarrage

L'environnement Local complet nécessite la virtualisation et les artefacts d'exécution décrits dans le [guide de développement](./DEVELOPMENT.md). Démarrer uniquement le serveur Web ne suffit pas.

```bash
git clone https://github.com/Zorker-Tech/Openlink.git
cd Openlink
git switch oss
corepack enable
pnpm install --frozen-lockfile
cp .env.example .env.local
# Vérifiez les paramètres nécessaires à votre environnement.
pnpm dev:local
```

Ne versionnez pas vos identifiants. Consultez [`.env.example`](./.env.example) pour les paramètres disponibles.

## Arborescence du dépôt

```text
Openlink/
├── app/                 Pages Web et routes API/BFF
├── components/          Composants d'interface communs
├── lib/, utils/         Logique du produit et utilitaires
├── services/            Services Agent, navigateur, connaissances et runtime
├── zorkerbase/          Plateforme de données et migrations
├── scripts/, deploy/    Cycle de vie et ressources de déploiement
├── docs/                Architecture, conception et exploitation
├── public/openlink/demo/ Vidéo et images de démonstration
├── license/             Licences et mentions de tiers
├── ARCHITECTURE.md      Limites du système
├── DEVELOPMENT.md       Guide de développement
└── LICENSE.md           Licence du dépôt
```

## Personnalisation et mises à jour

Conservez vos modifications sur une branche dédiée. Un worktree peut donner à cette branche son propre répertoire :

```bash
git fetch origin
git worktree add -b my-team/openlink-custom ../Openlink-custom origin/oss
```

Avec un seul répertoire, utilisez plutôt `git switch -c my-team/openlink-custom origin/oss`. Pour intégrer une mise à jour, exécutez `git fetch origin` puis `git merge origin/oss` sur votre branche et examinez les conflits. Un worktree ne réalise pas la migration à votre place.

## Évolutions prévues

Nous prévoyons d'extraire progressivement des composants OSS réutilisables en SDK, d'ajouter des fonctionnalités et de documenter une voie de mise à niveau. La publication en un clic et la connexion d'un domaine seront introduites progressivement dans l'édition OSS. Ces projets ne signifient pas qu'un SDK, une migration automatique ou ce parcours de publication sont déjà disponibles.

## Documentation et licence

[Développement](./DEVELOPMENT.md) · [Architecture](./ARCHITECTURE.md) · [Décisions d'architecture](./docs/architecture/README.md) · [Exploitation](./docs/operations/README.md)

Les issues et pull requests sont les bienvenues. Le dépôt est sous [licence Apache 2.0](./LICENSE.md). Avant de redistribuer des composants tiers, consultez les mentions regroupées dans [`license/`](./license/).
