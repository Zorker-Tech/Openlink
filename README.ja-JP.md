# OpenLink

**プロジェクトを中心に設計された、オープンソースの AI 開発アプリケーション。** OpenLink OSS は、実行、調査、拡張が可能な基盤アプリケーションです。独自の製品やワークフローに合わせて UI とサービスを変更できます。

**言語:** [English](./README.md) · [简体中文](./README.zh-CN.md) · [繁體中文](./README.zh-TW.md) · 日本語 · [한국어](./README.ko-KR.md) · [Français](./README.fr-FR.md) · [Deutsch](./README.de-DE.md) · [Русский](./README.ru-RU.md)

## 目次

- [デモ](#デモ)
- [主な機能](#主な機能)
- [はじめに](#はじめに)
- [リポジトリ構成](#リポジトリ構成)
- [カスタマイズと更新](#カスタマイズと更新)
- [今後の計画](#今後の計画)
- [ドキュメントとライセンス](#ドキュメントとライセンス)

## デモ

[![OpenLink のデモを見る](./public/openlink/demo/image/encoded-shot-4.png)](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)

[動画全編を見る](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)。動画と画像は説明用であり、一部の工程は時間を短縮して表現しています。

![プロジェクトを編集しプレビューを確認する画面](./public/openlink/demo/image/encoded-shot-7.png)

## 主な機能

**各プロジェクトが独立した完全なワークスペースです。** `/workspace` に専用のファイルと Git 履歴を持ち、永続的な実行環境、開発サービス、アプリケーション用バックエンドを備えます。チャットセッションは、そのプロジェクト内の別々のサンドボックスで動きます。VM バックエンドではプロジェクトごとに独立した Linux ゲストとカーネルを使用します。コンテナ互換バックエンドは隔離の強度が異なり、その区別が明示されます。[プロジェクト VM](./docs/architecture/ADR-0007-project-vm-runtime-boundaries.md) · [隔離クラス](./docs/architecture/ADR-0022-production-deployment-profiles-and-resource-governance.md)

- **作成と確認:** プロジェクトに紐づく Agent との対話、コードエディタ、ネイティブプレビュー、隔離された Chromium ブラウザを利用できます。[ブラウザ設計](./docs/architecture/ADR-0004-browser-runtime-surfaces.md)
- **プロジェクト専用バックエンド:** 各 Project VM で Auth、PostgreSQL、Realtime、Storage、Functions を含む Supabase 互換サービスを実行できます。データと秘密情報は制御プレーンから分離されます。[実行環境](./docs/architecture/standards/04-runtime-lifecycle.md)
- **マルチテナント:** ユーザー、組織、ワークスペース、プロジェクトの所有権を明確にし、サーバー側の認可確認と行レベルセキュリティを適用します。[データとセキュリティ](./docs/architecture/standards/05-data-and-security.md)
- **ローカル、Cloud、ナレッジ:** ローカルと Cloud は同じ契約に従いますが、ID とデータは独立しています。埋め込みモデルを設定すると、テナントでフィルタリングされたセマンティック検索が利用できます。[システム構成](./docs/architecture/standards/02-system-context.md)

**プロジェクトの公開:** 生成したプロジェクトを指定したサーバーまたはローカルホストへワンクリックで公開し、利用可能なドメインを自動割り当てする機能は、まだ OSS 版に導入されていません。今後の OSS リリースで段階的に導入します。OpenLink 自体のセルフホストとプロジェクトのプレビューは別の操作です。[インストールガイド](./docs/operations/self-hosted-installation.md)

## はじめに

完全なローカル環境には、[開発ガイド](./DEVELOPMENT.md)に記載の仮想化環境と事前に作成したランタイム成果物が必要です。Web のみの起動では全機能を利用できません。

```bash
git clone https://github.com/Zorker-Tech/Openlink.git
cd Openlink
git switch oss
corepack enable
pnpm install --frozen-lockfile
cp .env.example .env.local
# 環境に必要な設定を確認する
pnpm dev:local
```

認証情報を Git に追加しないでください。設定項目は [`.env.example`](./.env.example) を参照してください。

## リポジトリ構成

```text
Openlink/
├── app/                 Web ページと API/BFF
├── components/          共通 UI コンポーネント
├── lib/、utils/         製品ロジックとユーティリティ
├── services/            Agent、ブラウザ、ナレッジ、実行サービス
├── zorkerbase/          データ基盤とマイグレーション
├── scripts/、deploy/    ライフサイクルとデプロイ用ファイル
├── docs/                設計、アーキテクチャ、運用資料
├── public/openlink/demo/ デモ動画と画像
├── license/             サードパーティーのライセンス
├── ARCHITECTURE.md      システム境界
├── DEVELOPMENT.md       開発ガイド
└── LICENSE.md           本リポジトリのライセンス
```

## カスタマイズと更新

独自の変更は専用ブランチに保存してください。別ディレクトリが必要な場合は、そのブランチの worktree を作成します。

```bash
git fetch origin
git worktree add -b my-team/openlink-custom ../Openlink-custom origin/oss
```

単一ディレクトリなら `git switch -c my-team/openlink-custom origin/oss` を使用できます。更新時はカスタムブランチで `git fetch origin` と `git merge origin/oss` を実行し、競合を確認してください。worktree だけでアーキテクチャの移行が自動化されることはありません。

## 今後の計画

再利用可能な OSS コンポーネントの SDK 化、機能拡張、文書化されたアップグレード経路を計画しています。プロジェクトのワンクリック公開とドメイン接続も将来の方向性です。SDK、自動移行、完全な公開機能が現時点で利用可能であることを意味しません。

## ドキュメントとライセンス

[開発](./DEVELOPMENT.md) · [アーキテクチャ](./ARCHITECTURE.md) · [設計判断](./docs/architecture/README.md) · [運用](./docs/operations/README.md)

Issue と Pull Request を歓迎します。本リポジトリは [Apache License 2.0](./LICENSE.md) で提供されます。サードパーティー素材の再配布前に [`license/`](./license/) の条件も確認してください。
