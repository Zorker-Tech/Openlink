# OpenLink

**An open foundation for building with AI.** OpenLink is an open-source application for creating, iterating on, and previewing projects with an agent-powered workflow. This repository is the OpenLink OSS edition: a complete starting point that you can study, run, and extend for your own needs.

**Languages:** English · [简体中文](./README.zh-CN.md) · [繁體中文](./README.zh-TW.md) · [日本語](./README.ja-JP.md) · [한국어](./README.ko-KR.md) · [Français](./README.fr-FR.md) · [Deutsch](./README.de-DE.md) · [Русский](./README.ru-RU.md)

## Contents

- [See OpenLink](#see-openlink)
- [What this repository provides](#what-this-repository-provides)
- [What OpenLink can do](#what-openlink-can-do)
- [Get started](#get-started)
- [Repository map](#repository-map)
- [Build your own version](#build-your-own-version)
- [Where we are going](#where-we-are-going)
- [Documentation and contribution](#documentation-and-contribution)
- [License](#license)

## See OpenLink

[![Watch the OpenLink demo](./public/openlink/demo/image/encoded-shot-4.png)](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)

**[Watch the full demo video](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)** · A look at the prompt-to-preview workflow. The video and stills are illustrative; some steps are time-compressed.

| Start from an idea | Iterate on the result |
| --- | --- |
| ![An OpenLink prompt starting a project](./public/openlink/demo/image/encoded-shot-2.png) | ![An OpenLink conversation beside a project preview](./public/openlink/demo/image/encoded-shot-7.png) |

## What this repository provides

OpenLink OSS is the **base application**, not a restricted demo. You can use it as a foundation for your own product or workflow, adapt its interface and services, and develop more advanced capabilities around your requirements.

The codebase includes a Next.js web application, a backend-for-frontend, agent and browser services, project runtimes, and the supporting local infrastructure. These parts have distinct trust and process boundaries; the [architecture guide](./ARCHITECTURE.md) explains how they fit together. Some runtime components and dependencies have their own upstream licenses; see [License](#license).

## What OpenLink can do

OpenLink is built around **projects as complete workspaces**, not a single general-purpose assistant shared across unrelated work. Each project owns its files and Git history in `/workspace`, a durable runtime, development services, and a project-specific application backend. Chat sessions run as separate sandboxed workloads inside that project boundary. In VM mode, projects have separate Linux guests and therefore separate guest kernels; the optional container compatibility backend has a weaker, explicitly reported isolation class. See [ADR-0007](./docs/architecture/ADR-0007-project-vm-runtime-boundaries.md) and [ADR-0022](./docs/architecture/ADR-0022-production-deployment-profiles-and-resource-governance.md).

- **Build and inspect in one place.** Work with the agent in a project-linked chat, inspect files in the project code editor, and see changes in a native preview or an isolated Chromium browser. The agent and user share a browser protocol with scoped control. [Browser runtime design](./docs/architecture/ADR-0004-browser-runtime-surfaces.md)
- **Keep application data with the project.** Each Project VM can run its own Supabase-compatible backend, including Auth, PostgreSQL, Realtime, Storage, and Functions. Project secrets and data stay separate from OpenLink's control-plane database. [Runtime lifecycle](./docs/architecture/standards/04-runtime-lifecycle.md)
- **Support multiple tenants by design.** Users, organizations, workspaces, and projects have explicit ownership. Services recheck access, exposed tables use row-level security, and project and session capabilities are scoped rather than globally shared. [Data and security](./docs/architecture/standards/05-data-and-security.md)
- **Choose local or hosted operation.** Local mode starts an independent application data domain; Cloud mode uses the same contracts with separately managed identity and data. Self-hosted OpenLink installation and operations are documented for supported hosts. [Runtime lifecycle](./docs/architecture/standards/04-runtime-lifecycle.md) · [Self-hosted installation](./docs/operations/self-hosted-installation.md)
- **Work with project knowledge when configured.** The Knowledge Service ingests sources and performs tenant-filtered semantic search through a configured embedding provider and Zero. This capability requires a real embedding configuration. [System context](./docs/architecture/standards/02-system-context.md)

**Project publishing:** One-click deployment of a generated project to a chosen server or the local machine, with automatic assignment of a resolved domain, has not yet been introduced in the OSS edition. We will bring this capability into future OSS releases progressively. Self-hosting OpenLink and previewing a project are separate workflows. [Browser preview decision](./docs/architecture/ADR-0004-browser-runtime-surfaces.md) · [Operations guide](./docs/operations/README.md)

## Get started

For a full local development environment, prepare the host and runtime dependencies described in the [development guide](./DEVELOPMENT.md). In particular, local project VMs require a supported native virtualization setup, and the application uses prebuilt local runtime artifacts. A web-only process does not provide the complete application.

```bash
git clone https://github.com/Zorker-Tech/Openlink.git
cd Openlink
git switch oss
corepack enable
pnpm install --frozen-lockfile
cp .env.example .env.local
# Review .env.local and provide the values needed for your environment.
pnpm dev:local
```

The [example environment file](./.env.example) documents the available settings. Keep local credentials out of Git. If you are preparing runtime images or a self-hosted deployment, follow the dedicated instructions in [DEVELOPMENT.md](./DEVELOPMENT.md) and the [operations guide](./docs/operations/README.md) before starting the application.

## Repository map

```text
Openlink/
├── app/                 Next.js routes, UI entry points, and API/BFF routes
├── components/          Shared interface components
├── lib/ and utils/      Product logic and shared utilities
├── services/            Agent, browser, knowledge, and vendored runtimes
├── zorkerbase/          Application data platform and migrations
├── scripts/             Local lifecycle, build, and release tooling
├── deploy/              Deployment assets
├── docs/                Architecture, operations, and design documentation
├── public/openlink/demo/ Demo video and stills
├── license/             Third-party license and attribution collection
├── ARCHITECTURE.md      System boundaries and architectural rules
├── DEVELOPMENT.md       Development setup and conventions
└── LICENSE.md           Repository license
```

For detailed system boundaries, start with [ARCHITECTURE.md](./ARCHITECTURE.md). For implementation conventions, read [DEVELOPMENT.md](./DEVELOPMENT.md) and the [architecture standards index](./docs/architecture/standards/README.md).

## Build your own version

If you plan to customize OpenLink, put your changes on a **dedicated branch**. A separate worktree can give that branch its own directory, letting you keep a clean OSS checkout alongside your version:

```bash
git clone https://github.com/Zorker-Tech/Openlink.git
cd Openlink
git fetch origin
git worktree add -b my-team/openlink-custom ../Openlink-custom origin/oss
```

If you prefer one directory, create and switch to the branch directly:

```bash
git switch -c my-team/openlink-custom origin/oss
```

As OpenLink evolves, fetch the latest OSS changes and merge them into your customization branch:

```bash
git fetch origin
git switch my-team/openlink-custom
git merge origin/oss
```

This keeps your commits separate from upstream and gives you a clear place to resolve conflicts during major architecture changes. A worktree is a checkout of a branch; it does not replace the branch or make migrations automatic. Review release notes, migrations, and changed service contracts before upgrading a deployed installation.

## Where we are going

Today, this repository provides an application you can extend directly. As development continues, we plan to extract suitable OSS components into SDKs, expand the available functionality, progressively introduce one-click project publishing to a chosen server or local host with a resolved domain into OSS, and provide a documented upgrade path from this application foundation to those future components. These are **planned directions**; the SDKs, one-click project publishing, and automatic migration are not available in the OSS edition today.

Our aim is to let builders keep investing in their own OpenLink-based products while adopting future upstream improvements. Keeping custom changes on a separate branch now will make those transitions easier to review and integrate when the architecture changes.

## Documentation and contribution

- [Development guide](./DEVELOPMENT.md) — local workflow, host prerequisites, and editing conventions.
- [Architecture guide](./ARCHITECTURE.md) — system shape, ownership, and trust boundaries.
- [Architecture decisions](./docs/architecture/README.md) — accepted design decisions.
- [Operations guide](./docs/operations/README.md) — self-hosting and maintenance documentation.

Issues and pull requests are welcome. Before proposing a change across service boundaries, read the relevant architecture standards and describe the migration impact. Keep secrets, generated runtime state, and local artifacts out of contributions.

## License

OpenLink's repository license is [Apache License 2.0](./LICENSE.md). Third-party source and assets may carry separate terms; consult the notices collected in [`license/`](./license/) and the relevant component before redistribution.
