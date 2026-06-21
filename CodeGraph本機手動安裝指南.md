# CodeGraph 本機手動安裝指南

本指南提供在無法直接透過官方腳本安裝 CodeGraph CLI 工具時，於本機手動建置與安裝的詳細步驟。此方法適用於公司電腦有嚴格安裝限制，但允許自行編譯開源專案的環境。

## 1. 環境準備

CodeGraph 是一個基於 Node.js 和 TypeScript 的專案，因此您需要確保系統中已安裝以下工具：

*   **Node.js**: 建議使用 Node.js 20.0.0 或更高版本（`package.json` 中 `engines` 欄位指定 `>=20.0.0 <25.0.0`）。
*   **npm (Node Package Manager)** 或 **Yarn**: Node.js 安裝時通常會一併安裝 npm。您也可以選擇使用 Yarn。

您可以透過以下指令檢查 Node.js 和 npm 的版本：

```bash
node -v
npm -v
```

## 2. 取得原始碼

首先，您需要將 CodeGraph 的原始碼從 GitHub 倉庫複製到您的本機電腦。請開啟終端機或命令提示字元，執行以下指令：

```bash
git clone https://github.com/colbymchenry/codegraph.git
cd codegraph
```

這將會在當前目錄下建立一個名為 `codegraph` 的資料夾，並將所有原始碼下載到其中。

## 3. 安裝專案依賴

進入 `codegraph` 專案目錄後，使用 npm 或 Yarn 安裝專案所需的所有依賴套件：

```bash
npm install
# 或者使用 yarn
# yarn install
```

此步驟會根據 `package.json` 檔案中的定義，下載並安裝所有開發和運行時依賴。

## 4. 建置專案

依賴安裝完成後，執行專案的建置指令。這會將 TypeScript 原始碼編譯成 JavaScript，並準備好可執行的檔案：

```bash
npm run build
```

此指令會執行 `package.json` 中定義的 `build` 腳本，通常包括 TypeScript 編譯 (`tsc`) 和資產複製等步驟。建置完成後，可執行檔案會位於 `dist/bin/codegraph.js`。

## 5. 設定環境變數 (PATH)

為了方便在任何目錄下執行 `codegraph` 命令，建議將其可執行檔案的路徑加入到系統的 PATH 環境變數中。您可以選擇以下兩種方式：

### 方式一：建立符號連結 (推薦)

在您的使用者主目錄下建立一個本地 bin 目錄（如果不存在），並將 `codegraph.js` 建立符號連結到該目錄。然後確保該目錄已加入 PATH。

```bash
mkdir -p ~/.local/bin
ln -sf $(pwd)/dist/bin/codegraph.js ~/.local/bin/codegraph
```

接著，請確認 `~/.local/bin` 已經在您的 PATH 中。如果沒有，請將以下行添加到您的 shell 設定檔（例如 `~/.bashrc`, `~/.zshrc`, `~/.profile`）：

```bash
export PATH="$HOME/.local/bin:$PATH"
```

儲存檔案後，執行 `source ~/.bashrc` (或對應的設定檔) 使其生效，或開啟一個新的終端機視窗。

### 方式二：直接將 `dist/bin` 加入 PATH

您也可以直接將 `codegraph` 專案下的 `dist/bin` 目錄加入到 PATH 環境變數中。將以下行添加到您的 shell 設定檔：

```bash
export PATH="$(pwd)/dist/bin:$PATH"
```

同樣，儲存檔案後，執行 `source ~/.bashrc` (或對應的設定檔) 使其生效，或開啟一個新的終端機視窗。

## 6. 驗證安裝

開啟一個新的終端機視窗，然後執行以下指令來驗證 CodeGraph 是否已成功安裝並可執行：

```bash
codegraph --version
codegraph --help
```

如果顯示版本號和幫助資訊，則表示安裝成功。

## 7. 初始化專案

在您希望使用 CodeGraph 的專案目錄中，執行初始化指令來建立 `.codegraph/` 目錄並建置初始索引：

```bash
cd your-project-directory
codegraph init
```

## 8. 連接您的 AI 代理 (Agent)

CodeGraph 旨在與各種 AI 代理（如 Claude Code, Cursor, Codex CLI 等）整合。執行以下指令來連接您的代理：

```bash
codegraph install
```

這將會引導您完成配置過程，將 CodeGraph MCP 伺服器整合到您使用的代理中。

## 參考資料

1.  [colbymchenry/codegraph GitHub Repository](https://github.com/colbymchenry/codegraph)
2.  [Node.js 官方網站](https://nodejs.org/)
3.  [npm 官方網站](https://www.npmjs.com/)
