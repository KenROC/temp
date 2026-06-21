# OpenSpec Windows 本機手動安裝指南

本指南旨在提供在 Windows 環境下，無法直接透過 npm 全域安裝 OpenSpec CLI 工具時，進行本機手動建置與安裝的詳細步驟。此方法適用於公司電腦有嚴格安裝限制，但允許自行部署或編譯開源專案的環境。

OpenSpec 是一個專為 AI 編碼助理設計的規範驅動開發 (Spec-driven Development, SDD) 系統，旨在透過輕量級的規範層，確保人類與 AI 在編寫程式碼前對需求達成共識，從而提高開發效率和結果的可預測性 [1]。

## 1. 環境準備

OpenSpec 是一個基於 Node.js 和 TypeScript 的專案，並使用 pnpm 作為套件管理器。因此，您需要確保系統中已安裝以下工具：

*   **Node.js**: 專案要求 Node.js 20.19.0 或更高版本 (`package.json` 中 `engines` 欄位指定 `>=20.19.0`)。建議從 [Node.js 官方網站](https://nodejs.org/zh-cn/download/) 下載並安裝 LTS (長期支援) 版本。
*   **pnpm**: 由於專案使用 `pnpm-lock.yaml`，建議使用 pnpm 作為套件管理器。您可以透過 npm 安裝 pnpm：

    ```cmd
npm install -g pnpm
    ```

您可以透過以下指令檢查 Node.js 和 pnpm 的版本：

```cmd
node -v
pnpm -v
```

## 2. 取得原始碼

首先，您需要將 OpenSpec 的原始碼從 GitHub 倉庫複製到您的本機電腦。請開啟命令提示字元 (Command Prompt) 或 PowerShell，執行以下指令：

```cmd
git clone https://github.com/Fission-AI/OpenSpec.git
cd OpenSpec
```

這將會在當前目錄下建立一個名為 `OpenSpec` 的資料夾，並將所有原始碼下載到其中。

## 3. 安裝專案依賴

進入 `OpenSpec` 專案目錄後，使用 pnpm 安裝專案所需的所有依賴套件：

```cmd
pnpm install
```

此步驟會根據 `package.json` 和 `pnpm-lock.yaml` 檔案中的定義，下載並安裝所有開發和運行時依賴。

在安裝過程中，您可能會遇到 `[ERR_PNPM_IGNORED_BUILDS]` 警告，提示某些套件的建置腳本被忽略。這是 pnpm 的安全機制。如果遇到，您可以按照提示執行 `pnpm approve-builds`，然後選擇需要建置的套件（例如 `esbuild`），並確認批准。

## 4. 建置專案

依賴安裝完成後，執行專案的建置指令。這會將 TypeScript 原始碼編譯成 JavaScript，並準備好可執行的檔案：

```cmd
pnpm run build
```

此指令會執行 `package.json` 中定義的 `build` 腳本 (`node build.js`)，該腳本會使用 TypeScript 編譯器 (`tsc`) 將 `src` 目錄下的 TypeScript 檔案編譯到 `dist` 目錄 [2]。建置完成後，可執行檔案的入口點位於 `dist/cli/index.js`，而 CLI 的啟動腳本則在 `bin/openspec.js`。

## 5. 設定環境變數 (PATH)

為了方便在任何目錄下執行 `openspec` 命令，建議將其可執行檔案的路徑加入到系統的 PATH 環境變數中。您可以選擇以下兩種方式：

### 方式一：建立批次檔 (推薦)

由於 `bin/openspec.js` 是一個 Node.js 腳本，直接執行可能需要 `node` 命令。您可以建立一個批次檔 (Batch file) 來簡化執行：

1.  在您希望存放可執行檔的位置（例如 `C:\Users\您的使用者名稱\.local\bin`，如果不存在請自行建立）建立一個名為 `openspec.cmd` 的檔案。
2.  編輯 `openspec.cmd` 檔案，並加入以下內容：

    ```cmd
@echo off
node 
"%~dp0\..\openspec_repo\bin\openspec.js" %*
    ```

    請將 `"%~dp0\..\openspec_repo\bin\openspec.js"` 替換為您實際 `openspec.js` 檔案的絕對路徑。例如，如果您的 `openspec_repo` 在 `C:\Users\您的使用者名稱\Documents\openspec_repo`，則路徑應為 `"C:\Users\您的使用者名稱\Documents\openspec_repo\bin\openspec.js"`。

3.  **將批次檔目錄加入 PATH**: 參考 RTK 安裝指南中的步驟 4，將包含 `openspec.cmd` 的目錄 (例如 `C:\Users\您的使用者名稱\.local\bin`) 加入到系統的環境變數 `Path` 中。

### 方式二：直接使用 `node` 命令執行

如果您不想建立批次檔，也可以直接使用 `node` 命令來執行 `openspec`：

1.  **將 `openspec` 專案目錄加入 PATH**: 將 `openspec` 專案的根目錄 (例如 `C:\Users\您的使用者名稱\Documents\openspec_repo`) 加入到系統的環境變數 `Path` 中。這樣您就可以在任何位置使用 `node openspec.js` 來執行。

    ```cmd
    # 假設您在 openspec 專案根目錄下
    set PATH=%PATH%;%CD%\bin
    # 或者手動添加到系統環境變數 Path 中
    ```

## 6. 驗證安裝

開啟一個**新的**命令提示字元 (Command Prompt) 或 PowerShell 視窗，然後執行以下指令來驗證 OpenSpec 是否已成功安裝並可執行：

```cmd
openspec --version
openspec --help
```

如果顯示版本號和幫助資訊，則表示安裝成功。

## 7. 初始化專案

在您希望使用 OpenSpec 的專案目錄中，執行初始化指令來建立必要的 OpenSpec 檔案結構：

```cmd
cd your-project-directory
openspec init
```

這將會在您的專案中建立一個 `openspec/` 目錄，其中包含 `proposal.md`, `specs/`, `design.md`, `tasks.md` 等檔案，為您的 AI 協作開發提供結構化的工作流程 [1]。

## 8. 使用 OpenSpec

初始化專案後，您可以開始使用 OpenSpec 的命令與您的 AI 代理協作。例如，您可以告訴您的 AI：

```cmd
/opsx:propose <您想要建置的內容>
```

這將會根據您的提議，自動生成相關的規範文件。

## 參考資料

1.  [Fission-AI/OpenSpec GitHub Repository](https://github.com/Fission-AI/OpenSpec)
2.  [Fission-AI/OpenSpec build.js](https://github.com/Fission-AI/OpenSpec/blob/main/build.js)
3.  [Node.js 官方網站](https://nodejs.org/zh-cn/download/)
4.  [pnpm 官方網站](https://pnpm.io/)
