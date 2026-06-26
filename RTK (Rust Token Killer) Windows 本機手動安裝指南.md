# RTK (Rust Token Killer) Windows 本機手動安裝指南

本指南旨在提供在 Windows 環境下，無法直接透過自動化腳本安裝 RTK CLI 工具時，進行本機手動安裝的詳細步驟。此方法適用於公司電腦有嚴格安裝限制，但允許自行部署或編譯開源專案的環境。

RTK (Rust Token Killer) 是一個高效能的 CLI 代理工具，旨在透過過濾和壓縮命令輸出，顯著減少大型語言模型 (LLM) 的 Token 消耗，節省 60-90% 的 Token 使用量 [1]。

## 1. 環境準備

根據您選擇的安裝方式，您可能需要準備以下環境：

### 方式一：使用預編譯二進位檔 (推薦)

此方式最為簡便，不需要安裝 Rust 編譯環境，只需確保您有一個可以將可執行檔加入系統 PATH 的位置。

### 方式二：從原始碼建置 (需要 Rust 編譯環境)

如果您選擇從原始碼建置，則需要安裝 **Rust 編譯工具鏈**。請前往 [Rust 官方網站](https://www.rust-lang.org/tools/install) 下載並安裝 `rustup`。`rustup` 是 Rust 的官方工具鏈安裝程式，它會安裝 `rustc` (Rust 編譯器) 和 `cargo` (Rust 的套件管理器和建置工具)。

安裝完成後，您可以在命令提示字元 (Command Prompt) 或 PowerShell 中執行以下指令來驗證安裝：

```cmd
rustc --version
cargo --version
```

## 2. 取得原始碼

首先，您需要將 RTK 的原始碼從 GitHub 倉庫複製到您的本機電腦。請開啟命令提示字元 (Command Prompt) 或 PowerShell，執行以下指令：

```cmd
git clone https://github.com/rtk-ai/rtk.git
cd rtk
```

這將會在當前目錄下建立一個名為 `rtk` 的資料夾，並將所有原始碼下載到其中。

## 3. 安裝方式

### 方式一：使用預編譯二進位檔 (推薦)

這是 Windows 環境下最推薦的安裝方式，因為它避免了編譯的複雜性。

1.  **下載最新版本**: 前往 RTK 的 [GitHub Releases 頁面](https://github.com/rtk-ai/rtk/releases)。
2.  **尋找 Windows 版本**: 找到最新版本下的 `rtk-x86_64-pc-windows-msvc.zip` 檔案並下載。
3.  **解壓縮**: 將下載的 `zip` 檔案解壓縮到您選擇的目錄，例如 `C:\Users\您的使用者名稱\.local\bin`。解壓縮後，您會找到 `rtk.exe` 檔案。
4.  **將 `rtk.exe` 加入 PATH**: 為了能在任何目錄下執行 `rtk` 命令，您需要將包含 `rtk.exe` 的目錄加入到系統的環境變數 `Path` 中。步驟如下：
    *   在 Windows 搜尋欄中輸入「環境變數」，然後選擇「編輯系統環境變數」。
    *   點擊「環境變數」按鈕。
    *   在「使用者變數」或「系統變數」區塊中，找到 `Path` 變數並選中，然後點擊「編輯」。
    *   點擊「新建」，然後輸入 `rtk.exe` 所在的完整路徑 (例如 `C:\Users\您的使用者名稱\.local\bin`)。
    *   點擊「確定」關閉所有視窗。

### 方式二：從原始碼建置 (需要 Rust 編譯環境)

如果您已經安裝了 Rust 工具鏈，可以選擇從原始碼建置 RTK：

1.  **進入專案目錄**: 確保您已在終端機中進入 `rtk` 專案的根目錄 (即您執行 `git clone` 後的 `rtk` 資料夾)。
2.  **建置專案**: 執行以下命令來編譯專案。`--release` 標誌會生成優化過的可執行檔。

    ```cmd
cargo build --release
    ```

    **注意**：RTK 的 `build.rs` 檔案包含針對 Windows 的特定配置 (`cargo:rustc-link-arg=/STACK:8388608`)，以解決在 Windows 上啟動時可能出現的堆疊溢位問題 [2]。這會由 `cargo` 自動處理。

3.  **安裝可執行檔**: 建置成功後，可執行檔 `rtk.exe` 會位於 `target\release\` 目錄下。您可以將此檔案複製到您希望的位置，例如 `C:\Users\您的使用者名稱\.local\bin`。
4.  **將 `rtk.exe` 加入 PATH**: 參考方式一的步驟 4，將包含 `rtk.exe` 的目錄加入到系統的環境變數 `Path` 中。

## 4. 驗證安裝

開啟一個**新的**命令提示字元 (Command Prompt) 或 PowerShell 視窗，然後執行以下指令來驗證 RTK 是否已成功安裝並可執行：

```cmd
rtk --version
rtk gain
```

如果 `rtk --version` 顯示版本號，且 `rtk gain` 顯示 Token 節省統計資訊，則表示安裝成功。如果 `rtk gain` 失敗，可能是因為您安裝了另一個同名的 Rust 專案 (Rust Type Kit)。在這種情況下，請參考 `rtk-ai/rtk` 倉庫的 `INSTALL.md` 文件中的「Uninstall Wrong RTK」部分進行處理 [1]。

## 5. 專案初始化與 AI 代理連接

RTK 旨在與各種 AI 代理（如 Claude Code, Gemini CLI, Codex CLI 等）整合。為了讓 RTK 發揮作用，您需要初始化它並連接到您的 AI 工具。

### 推薦：全域 Hook 優先設定

這會為所有專案啟用 RTK，並自動重寫命令：

```cmd
rtk init -g
```

此命令會引導您完成設定，包括安裝 Hook 到 `~/.claude/hooks/rtk-rewrite.sh` (即使在 Windows 上，RTK 也會嘗試配置類似的機制或提供指示)，建立 `~/.claude/RTK.md`，並可能提示您修補 `settings.json`。請在提示時回答 `y` 以自動修補。

### 替代方案：本機專案設定

如果您只想在特定專案中使用 RTK，可以在該專案目錄中執行：

```cmd
cd your-project-directory
rtk init
```

這會在專案目錄中建立一個 `CLAUDE.md` 檔案，其中包含 RTK 的使用說明。

## 6. Windows 特有考量：WSL (Windows Subsystem for Linux)

RTK 的完整 Hook 系統在 Linux 環境下運作最為原生和順暢。如果您在 Windows 上遇到任何問題，或者希望獲得最佳體驗，強烈建議您考慮使用 [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/zh-tw/windows/wsl/install) 來安裝和運行 RTK。在 WSL 環境中，您可以按照 Linux/macOS 的快速安裝步驟進行操作 [1]。

## 參考資料

1.  [rtk-ai/rtk GitHub Repository](https://github.com/rtk-ai/rtk)
2.  [rtk-ai/rtk build.rs](https://github.com/rtk-ai/rtk/blob/develop/build.rs)
3.  [Rust 官方網站](https://www.rust-lang.org/tools/install)


https://yazelin.github.io/ipas-ai-quiz/