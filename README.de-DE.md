# OpenLink

**Eine quelloffene KI-Entwicklungsanwendung, bei der jedes Projekt im Mittelpunkt steht.** OpenLink OSS ist eine Basisanwendung, die Sie ausführen, untersuchen und für Ihr eigenes Produkt oder Ihren Arbeitsablauf erweitern können.

**Sprachen:** [English](./README.md) · [简体中文](./README.zh-CN.md) · [繁體中文](./README.zh-TW.md) · [日本語](./README.ja-JP.md) · [한국어](./README.ko-KR.md) · [Français](./README.fr-FR.md) · Deutsch · [Русский](./README.ru-RU.md)

## Inhalt

- [Demo](#demo)
- [Funktionen](#funktionen)
- [Schnellstart](#schnellstart)
- [Verzeichnisübersicht](#verzeichnisübersicht)
- [Anpassungen und Aktualisierungen](#anpassungen-und-aktualisierungen)
- [Ausblick](#ausblick)
- [Dokumentation und Lizenz](#dokumentation-und-lizenz)

## Demo

[![OpenLink-Demo ansehen](./public/openlink/demo/image/encoded-shot-4.png)](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)

[Vollständiges Video ansehen](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4). Video und Bilder veranschaulichen den Arbeitsablauf; einige Schritte wurden zeitlich verkürzt.

![Projektbearbeitung und Vorschau in OpenLink](./public/openlink/demo/image/encoded-shot-7.png)

## Funktionen

**Jedes Projekt ist ein vollständiger Arbeitsbereich.** Es besitzt Dateien und Git-Verlauf unter `/workspace`, eine dauerhafte Laufzeitumgebung, Entwicklungsdienste und ein eigenes Anwendungs-Backend. Chat-Sitzungen laufen in getrennten Sandboxes innerhalb der Projektgrenze. Im VM-Modus hat jedes Projekt eine eigene Linux-Gastmaschine mit eigenem Gast-Kernel. Der optionale Container-Kompatibilitätsmodus hat eine schwächere, klar ausgewiesene Isolationsklasse.[Projekt-VM](./docs/architecture/ADR-0007-project-vm-runtime-boundaries.md) · [Isolationsklassen](./docs/architecture/ADR-0022-production-deployment-profiles-and-resource-governance.md)

- **Erstellen und prüfen:** Nutzen Sie den Agenten im projektgebundenen Chat, bearbeiten Sie Dateien im Code-Editor und prüfen Sie Ergebnisse in der nativen Vorschau oder in einem isolierten Chromium-Browser.[Browser-Konzept](./docs/architecture/ADR-0004-browser-runtime-surfaces.md)
- **Eigenes Backend pro Projekt:** Jede Project VM kann Supabase-kompatible Dienste für Auth, PostgreSQL, Realtime, Storage und Functions ausführen. Daten und Geheimnisse bleiben von der OpenLink-Steuerung getrennt.[Laufzeitmodell](./docs/architecture/standards/04-runtime-lifecycle.md)
- **Mandantenfähigkeit:** Benutzer, Organisationen, Arbeitsbereiche und Projekte haben eindeutige Eigentümer. Dienste prüfen Zugriffe erneut; veröffentlichte Tabellen nutzen Row-Level Security.[Daten und Sicherheit](./docs/architecture/standards/05-data-and-security.md)
- **Local, Cloud und Wissenssuche:** Local und Cloud verwenden dieselben Verträge, aber getrennte Identitäten und Daten. Mit konfiguriertem Embedding-Anbieter steht mandantengefilterte semantische Suche zur Verfügung.[Systemübersicht](./docs/architecture/standards/02-system-context.md)

**Projektveröffentlichung:** Die Veröffentlichung eines erstellten Projekts auf einem gewählten Server oder dem lokalen Rechner per Klick, einschließlich automatischer Zuweisung einer auflösbaren Domain, wurde in der OSS-Version noch nicht eingeführt. Diese Funktion wird in künftigen OSS-Versionen schrittweise ergänzt. Das Self-Hosting von OpenLink und die Projektvorschau sind davon getrennte Abläufe.[Self-Hosting](./docs/operations/self-hosted-installation.md)

## Schnellstart

Für die vollständige lokale Umgebung benötigen Sie die Virtualisierung und vorbereiteten Laufzeitartefakte aus dem [Entwicklungsleitfaden](./DEVELOPMENT.md). Der Webprozess allein stellt nicht die gesamte Anwendung bereit.

```bash
git clone https://github.com/Zorker-Tech/Openlink.git
cd Openlink
git switch oss
corepack enable
pnpm install --frozen-lockfile
cp .env.example .env.local
# Prüfen Sie die für Ihre Umgebung nötigen Einstellungen.
pnpm dev:local
```

Speichern Sie Zugangsdaten nicht in Git. Die Einstellungen stehen in [`.env.example`](./.env.example).

## Verzeichnisübersicht

```text
Openlink/
├── app/                 Webansichten und API/BFF-Routen
├── components/          Gemeinsame UI-Komponenten
├── lib/, utils/         Produktlogik und Hilfsfunktionen
├── services/            Agent-, Browser-, Wissens- und Laufzeitdienste
├── zorkerbase/          Datenplattform und Migrationen
├── scripts/, deploy/    Lebenszyklus und Bereitstellungsdateien
├── docs/                Architektur-, Entwurfs- und Betriebsdokumente
├── public/openlink/demo/ Demovideo und Bilder
├── license/             Lizenzen und Hinweise Dritter
├── ARCHITECTURE.md      Systemgrenzen
├── DEVELOPMENT.md       Entwicklungsleitfaden
└── LICENSE.md           Repository-Lizenz
```

## Anpassungen und Aktualisierungen

Bewahren Sie eigene Änderungen auf einem separaten Branch auf. Ein Worktree gibt diesem Branch bei Bedarf ein eigenes Verzeichnis:

```bash
git fetch origin
git worktree add -b my-team/openlink-custom ../Openlink-custom origin/oss
```

Bei nur einem Verzeichnis verwenden Sie `git switch -c my-team/openlink-custom origin/oss`. Holen Sie spätere OSS-Änderungen mit `git fetch origin` und führen Sie auf Ihrem Branch `git merge origin/oss` aus. Prüfen Sie Konflikte und Migrationen; ein Worktree erledigt dies nicht automatisch.

## Ausblick

Wir möchten wiederverwendbare OSS-Komponenten schrittweise als SDKs bereitstellen, Funktionen ausbauen und einen dokumentierten Upgrade-Pfad anbieten. Die Veröffentlichung per Klick samt Domain-Anbindung soll ebenfalls schrittweise in OSS eingeführt werden. Diese Pläne sind keine Aussage, dass SDKs, automatische Migration oder der vollständige Veröffentlichungsablauf bereits verfügbar sind.

## Dokumentation und Lizenz

[Entwicklung](./DEVELOPMENT.md) · [Architektur](./ARCHITECTURE.md) · [Architekturentscheidungen](./docs/architecture/README.md) · [Betrieb](./docs/operations/README.md)

Issues und Pull Requests sind willkommen. Das Repository steht unter der [Apache-Lizenz 2.0](./LICENSE.md). Prüfen Sie vor der Weitergabe von Drittanbieterkomponenten auch die Hinweise in [`license/`](./license/).
