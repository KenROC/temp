## Subagent Usage

**何謂 noisy job：**
任何會產生大量輸出、佔用上下文空間的命令，例如：
- 測試輸出（如 `vitest run` 印出 400 行 passing 結果）
- 建置輸出（`tsc`、`npm run build` 等）
- `git log`、`git diff` 等歷史輸出
- 大型日誌檔案掃描

**何時啟動 subagent：**
1. 處理 noisy jobs - 避免大量輸出永久併入主會話上下文
2. 產出量大但主會話只需要關鍵摘要
3. 工作可以獨立進行，不需要與當前上下文交互
4. 避免主會話上下文被 noisy output 污染

**何時避免啟動 subagent：**
- **小型、快速工作**（如單一檔案修復）—— subagent 開銷可能超過收益
- **需要深入了解主會話上下文**的工作—— subagent 看不到歷史對話
- **需要即時協作**的情境—— subagent 會打斷對話流程

**成本權衡說明：**
- subagent 有自己的 context window（不含主會話歷史）
- 可能需要重新讀取已知資訊（小任務的額外開銷）
- 但能避免 noisy output 永久併入主會話，長期來往通常更省 token

**實務原則：**
對於「produces a lot of output you don't need to keep」的任務，優先啟動 subagent（建議 model: haiku 或 sonnet）；小型工作直接執行，勿啟動 subagent 以免產生額外開銷。