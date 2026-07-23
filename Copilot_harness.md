這是我唯一一次使用 GPT-5.6 Terra Max 的機會。這個 session 結束後，日常任務將全程由 VS Code 的 Auto 路由機制長期運作。你的任務：把你的判斷力轉成一套「語言無關、可移植任何專案」的制度檔案，讓之後每一個日常 Auto session 都因此變強。

核心限制：
- 不要產生任何程式碼檔案。只產出制度檔（.github/ 與 .agents/ 下的 Markdown/YAML）。
- 所有規則必須語言無關。需要語言特定資訊的地方，使用佔位符：{{LANG}}（語言名稱）、{{FRAMEWORK}}（框架）、{{TEST_CMD}}（測試指令）、{{BUILD_CMD}}（建置指令）、{{LINT_CMD}}（lint 指令）、{{SRC_DIR}}（原始碼目錄）。
- 支援多語言：必須相容 C#、VB .NET、React、Angular、Java、Vue 等主流框架。
- 不要在任何 .agent.md 寫死 model 欄位。全程配合 VS Code Auto 路由。
- 日常任務最大的痛點是「Context 太大導致失敗」。所有制度必須以「極簡 Context、隔離 Tool Outputs、適時斷點交接」為最高指導原則。

作業規則：
1. 自主作業。開場不准提問，直接執行，之後不再停下來等我。
2. 價值排序＋隨做隨寫。每完成一項立刻寫進檔案再做下一項。
3. 改任何既有檔案前先留 .bak 備份副本。
4. 你的讀者是未來的 Auto session。規則要具體、可執行、有判準與範例。
- 嚴禁幻覺：所有規則必須基於實際工具能力。

交付清單：
A. 核心指令（.github/copilot-instructions.md）：
   - 控制在 60 行以內。
   - 內容：路由表、Auto 路由引導守則、Context 紀律摘要。
   - 移植時強制執行 $project-init 以確保環境變數正確。

B. 委派守則（.github/instructions/10-dispatch.instructions.md）：
   - 禁止親自執行大量掃描，必須派 worker。
   - Worker 回報：只回報結論、行號與路徑，禁止印出超過 10 行的原始碼。
   - 升降級：worker 連錯兩次 → 提示使用者重新評估。
   - 驗收不自驗：一律派 reviewer。

C. Context 防爆紀律（.github/instructions/20-context-discipline.instructions.md）：
   - 禁止印出超過 20 行的程式碼。
   - Checkpoint：每修改 2 個檔案或對話超過 10 輪，必須寫入 handoff.md 並提示開新 Chat。
   - 長篇分析必須寫入檔案再引用路徑。

D. Custom Agents（.github/agents/）：
   - worker.agent.md：工具限縮到讀取、搜尋與編輯。指示 < 40 行。
   - reviewer.agent.md：唯讀工具。輸出格式為「✅/❌ + 一句話理由」。

E. 判斷力 Checklist（.agents/skills/judgment/SKILL.md）：
   - 至少 6 條判準，每條附正例與反例。

F. 防爆版派工 Skills（.agents/skills/delegate-*/SKILL.md）：
   - delegate-implement/refactor/review：內建 Context Checkpoint 與強制派工機制。

G. 移植初始化 Skill（.agents/skills/project-init/SKILL.md）：
   - 自動替換 {{LANG}}、{{FRAMEWORK}} 等佔位符。對於 C#/.NET 專案，自動識別 {{SRC_DIR}} 為專案根目錄或 src。

H. 維護協議與收尾：
   - 踩坑教訓寫回 Known Pitfalls。
   - 收尾必做：fresh-context 審查、read-back 驗證、SUMMARY.md（兩步移植法）。

誠實條款：不確定的事就標註，不要編造。