# OpenLink

**以專案為中心的開源 AI 開發應用。** OpenLink OSS 提供可自行執行及擴充的基礎應用。你可以依需求修改介面、服務與工作流程，打造自己的產品。

**語言：** [English](./README.md) · [简体中文](./README.zh-CN.md) · 繁體中文 · [日本語](./README.ja-JP.md) · [한국어](./README.ko-KR.md) · [Français](./README.fr-FR.md) · [Deutsch](./README.de-DE.md) · [Русский](./README.ru-RU.md)

## 目錄

- [示範](#示範)
- [主要能力](#主要能力)
- [開始使用](#開始使用)
- [原始碼索引](#原始碼索引)
- [自訂與升級](#自訂與升級)
- [發展方向](#發展方向)
- [文件與授權](#文件與授權)

## 示範

[![觀看 OpenLink 示範](./public/openlink/demo/image/encoded-shot-4.png)](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)

[觀看完整影片](./public/openlink/demo/OpenLink-4K60-Paced-001.mp4)。影片與圖片為示意內容，部分步驟經過時間壓縮。

![在 OpenLink 中迭代專案並查看預覽](./public/openlink/demo/image/encoded-shot-7.png)

## 主要能力

**每個專案都是完整的工作空間。** 專案有自己的 `/workspace` 檔案與 Git 紀錄、持久化執行環境、開發服務及應用後端。各對話工作階段在專案內以獨立沙箱執行。選用 VM 後端時，每個專案擁有獨立的 Linux 虛擬機器及客體核心；容器相容後端的隔離等級較低，系統會明確標示。[專案 VM 設計](./docs/architecture/ADR-0007-project-vm-runtime-boundaries.md) · [執行環境類別](./docs/architecture/ADR-0022-production-deployment-profiles-and-resource-governance.md)

- **建構與檢查：** 在專案對話中使用 Agent，透過程式碼編輯器、原生預覽與隔離的 Chromium 瀏覽器檢查成果。[瀏覽器設計](./docs/architecture/ADR-0004-browser-runtime-surfaces.md)
- **獨立的專案後端：** Project VM 可執行自己的 Supabase 相容服務，包含 Auth、PostgreSQL、Realtime、Storage 與 Functions；資料及機密資訊不與控制平面混用。[執行環境](./docs/architecture/standards/04-runtime-lifecycle.md)
- **原生多租戶：** 使用者、組織、工作空間和專案有明確的所有權；服務端驗證權限，資料表使用資料列層級安全性。[資料與安全](./docs/architecture/standards/05-data-and-security.md)
- **本機、雲端及知識服務：** 本機與 Cloud 採用相同契約，但身份與資料各自獨立；設定嵌入模型後，可使用知識匯入與經租戶篩選的語意搜尋。[系統架構](./docs/architecture/standards/02-system-context.md)

**專案發布：** 一鍵將產出的專案部署至指定伺服器或本機，並自動指派已解析網域的能力尚未引入 OSS 版本；後續會逐步引入。OpenLink 自行託管與專案預覽是不同流程。[安裝文件](./docs/operations/self-hosted-installation.md)

## 開始使用

完整本機環境需要[開發指南](./DEVELOPMENT.md)所述的虛擬化設定與預先準備的執行產物；單獨啟動 Web 服務並不等於完整應用。

```bash
git clone https://github.com/Zorker-Tech/Openlink.git
cd Openlink
git switch oss
corepack enable
pnpm install --frozen-lockfile
cp .env.example .env.local
# 檢查並填入環境所需設定
pnpm dev:local
```

請勿將本機憑證提交至 Git。設定方式見 [`.env.example`](./.env.example)。

## 原始碼索引

```text
Openlink/
├── app/                 Web 頁面及 API/BFF
├── components/          共用介面元件
├── lib/、utils/         產品邏輯及工具
├── services/            Agent、瀏覽器、知識及執行服務
├── zorkerbase/          資料平台與遷移
├── scripts/、deploy/    執行生命週期與部署資源
├── docs/                架構、設計及維運文件
├── public/openlink/demo/ 示範影片與圖片
├── license/             第三方授權及聲明
├── ARCHITECTURE.md      系統邊界
├── DEVELOPMENT.md       開發指南
└── LICENSE.md           專案授權條款
```

## 自訂與升級

建議將自訂程式碼保存在獨立分支；需要獨立目錄時，再為該分支建立 worktree：

```bash
git fetch origin
git worktree add -b my-team/openlink-custom ../Openlink-custom origin/oss
```

若只使用一個目錄，可改用 `git switch -c my-team/openlink-custom origin/oss`。取得上游更新後，在自訂分支執行 `git fetch origin` 及 `git merge origin/oss`，檢查並解決衝突。worktree 本身不會自動遷移架構，也不能取代分支。

## 發展方向

我們計畫逐步將可重用的 OSS 元件整理為 SDK，擴充更多功能，並提供文件化的升級途徑；專案一鍵發布及網域接入也屬於後續方向。這些規劃不表示目前已有 SDK、自動遷移或完整的一鍵發布功能。

## 文件與授權

[開發指南](./DEVELOPMENT.md) · [架構規範](./ARCHITECTURE.md) · [架構決策](./docs/architecture/README.md) · [維運文件](./docs/operations/README.md)

歡迎提交 Issue 與 Pull Request。OpenLink 採用 [Apache License 2.0](./LICENSE.md)；第三方元件可能有不同條款，重新散布前請查看 [`license/`](./license/) 中的聲明。
