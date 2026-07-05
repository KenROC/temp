這是我唯一一次在這個 repo 用旗艦模型（Claude Opus 4.6）跑長 session 立制度的機會。這個 session 結束後，日常工作將由低倍率模型（Claude Haiku 4.5、Gemini 3.1 Flash、GPT 5.2 Codex）與主力模型（Claude Sonnet 4.6、GPT 5.3 Codex、Gemini 3.1 Pro）長期運作。所以你的任務只有一個：把你的判斷力轉成可長期沿用的制度與檔案，讓之後每一個較弱模型的 session 都因此變強。用這個 session 立制度，不要拿去執行日常任務。

作業規則（先讀完再動手）：
1. 自主作業。環境能查的自己查：.github/copilot-instructions.md、AGENTS.md、.github/instructions/、.github/agents/、.github/prompts/、.github/skills/、.github/hooks/、已設定的 MCP servers，以及用 memory 工具查看 /memories/ 與 /memories/repo/ 的既有內容。開場最多問我一批問題，五題以內，之後不再停下來等我。
2. 價值排序＋隨做隨寫。先做價值最高的產出，每完成一項立刻落檔再做下一項。這個 session 隨時可能中斷，中斷時已落檔的就是我拿到的全部。
3. 改任何既有檔案前先在同目錄留 .bak 備份副本。新內容寫成新檔：.github/copilot-instructions.md 只放精簡路由與跨檔案通則，具體規則拆進 .github/instructions/ 下的 *.instructions.md 並正確設定 applyTo glob（全域用 "**"，語言限定用如 "**/*.py"）。注意多份 instructions 是無序合併載入的，每個檔案必須自足、不得依賴其他檔案的載入順序。
4. 你的讀者是較弱的模型（Haiku 4.5、Gemini 3.1 Flash 等級）。你自己不需要被逐步指揮，但寫給它們的規則要具體、可執行、有判準與範例；抽象要求（例如「保持高品質」）等於沒寫。
5. 產出不可依賴只有旗艦模型才做得到的能力，全部要在 Sonnet 4.6 等級就跑得動，關鍵流程要在 Haiku 4.5 等級也不出軌。

環境事實（寫檔時以此為準，不要憑印象改）：
- 可用模型分三檔：旗艦＝Claude Opus 4.6、GPT 5.4；主力＝Claude Sonnet 4.6、Gemini 3.1 Pro、GPT 5.3 Codex；經濟＝Claude Haiku 4.5、Gemini 3.1 Flash、GPT 5.2 Codex。沒有 effort 參數，推理深度只能靠換模型。
- 計費：每個 user prompt 消耗 premium request/credits 乘以模型倍率；agent 自主的 tool calls 不計費。
- Custom agent（.github/agents/*.agent.md）的 frontmatter 可寫死 model（單一或優先序陣列，格式如 "Claude Haiku 4.5 (copilot)"）、tools 白名單、agents 委派白名單、handoffs。custom agent 作為 subagent 執行時會覆蓋主對話的模型——這是本環境異質調度的核心機制。
- Subagent 由主 agent 經 agent/runSubagent 工具派出，context 與主對話隔離；預設 subagent 不能再開 subagent。
- Prompt files（.github/prompts/*.prompt.md）以 /名稱 呼叫，frontmatter 可指定 agent 與 model，body 可用 ${input:變數名:提示文字} 填空。
- Memory：user scope（/memories/，前 200 行每 session 自動載入，量要克制）、repo scope（/memories/repo/）、session scope（結束即清除，不可放制度）。長期制度一律以 .github/ 下的檔案為準，memory 只放輔助性偏好。

交付清單（按價值排序執行）：
A. 快速診斷：這個 repo 目前最浪費 premium requests、最容易失焦、最容易出錯的前三名，各附具體修法（例如：旗艦模型拿去跑批次改檔、instructions 檔互相矛盾、該派 subagent 隔離的掃描工作塞爆主對話 context）。先落檔為 .github/instructions/00-diagnosis.instructions.md（applyTo 設 "**"），供後面所有產出引用。
B. 重寫 copilot-instructions.md：最高槓桿的檔案，未來每個 session 都會載入。收斂重複規則、刪過時內容、把語言與目錄限定的長內容抽成 .github/instructions/ 引用檔。套用「弱模型需要明確、強模型需要留白」的寫法。若 repo 有 AGENTS.md，讓兩者分工明確（AGENTS.md 放跨工具通用事實，copilot-instructions.md 放 Copilot 專屬流程），不得重複。
C. 異質模型調度制度（.github/agents/ 下的 custom agents ＋一份調度規則檔）：
   - 建 worker.agent.md：model 設 ["Claude Haiku 4.5 (copilot)", "Gemini 3.1 Flash (copilot)"]，工具限縮到讀取、搜尋與編輯，職責是大量讀取、掃 repo、批次改檔、資訊蒐集。
   - 建 planner.agent.md：model 設 ["Claude Opus 4.6 (copilot)", "GPT 5.4 (copilot)"]，唯讀工具，職責是架構決策與實作計畫，handoffs 指向 implementer。
   - 建 implementer.agent.md：model 設 ["Claude Sonnet 4.6 (copilot)", "GPT 5.3 Codex (copilot)"]，完整編輯工具，handoffs 指向 reviewer。
   - 建 reviewer.agent.md：model 設主力檔、唯讀工具、disable-model-invocation 設 false 讓其可被派為 subagent，職責是以驗收條件逐條核對。
   - 調度規則寫進 .github/instructions/10-dispatch.instructions.md：指揮官不下場（大量讀取、掃 repo、查網頁、批次改檔一律派 worker subagent，主對話只進結論）；派工三件套（目標與動機、驗收條件、回報格式）；回報合約（subagent 只回結論與 檔案:行號，長產物落檔傳路徑）；升降級路徑（worker 錯一次直接升 implementer、implementer 同一子任務連錯兩次帶完整失敗軌跡升 planner、planner 解出的模式寫成規則或 prompt file 後降回 worker 批次套用、同一件事最多重試兩輪）；驗證不自驗（驗收一律派 context 乾淨的 reviewer subagent：檔案用 read-back、程式碼用測試或實跑、高風險判斷平行派兩個不同模型的 subagent 各自獨立作答後比對共識選優）。
D. 判斷力外化（.github/instructions/20-judgment.instructions.md）：把只有旗艦模型才擅長的判斷，寫成 Haiku 等級可執行的 rubric 與 checklist。至少涵蓋：何時該升級模型（依上面三檔梯隊寫觸發條件）、何時算真的完成、何時該停下來問使用者、什麼訊號代表方向錯了該換路而非重試、品質底線怎麼驗。每條判準附一個正例和一個反例。
E. 派工模板（.github/prompts/ 下的 prompt files）：常見任務型態各一份，含驗收條件與回報格式的 ${input:} 填空，並在 frontmatter 用 agent 欄位綁定 C 建立的對應 custom agent：delegate-search.prompt.md（綁 worker）、delegate-implement.prompt.md（綁 implementer）、delegate-refactor.prompt.md（綁 implementer）、delegate-research.prompt.md（綁 worker）、delegate-review.prompt.md（綁 reviewer）。
F. 維護協議（.github/instructions/30-maintenance.instructions.md）：未來的弱模型怎麼安全地更新以上檔案。哪些可以自行改（例如追加踩坑教訓到指定段落）、哪些動之前要先問使用者（調度梯隊與判斷 rubric 的判準本身、agent 檔的 model 欄位）、每次踩坑後教訓寫回哪個檔案哪個段落、用什麼格式、累積多長要精簡、改前先留 .bak。同時規定 memory 的使用紀律：偏好類記 user memory、repo 事實記 repo memory、絕不把制度只存在 session memory。
G. 給未來 session 的信（.github/instructions/99-letter.md，注意：故意不用 .instructions.md 副檔名，避免被自動載入吃 context，需要時手動附加）：三件我沒問、但你認為對這個環境最重要的事，加上你認為這套制度最可能的退化方式與預防法。

收尾（必做，不可省略）：
1. 派一個 context 乾淨的 reviewer subagent 對抗審查全部產出：找規則互相打架、路徑或 frontmatter 欄位錯誤（模型名、applyTo glob、tools 名稱以官方文件實際格式為準）、弱模型會誤讀的模糊語句，修完為止。若環境裝有 Chat Customizations Evaluations 擴充，另外對每個檔案跑 /analyze-prompt 並修復 Problems 面板列出的診斷。
2. 逐一 read-back 驗證每個檔案確實落地、內容完整、frontmatter 是合法 YAML。
3. 給我一頁總結（落檔為 SUMMARY.md）：改了什麼、為什麼、明天開始怎麼用（日常開對話該選哪個模型、五個 /delegate-* 指令何時用、四個 custom agent 的 handoff 流程圖）。
4. 如果 context 快用完：立刻停下手上的產出，先完成收尾前三步，把未完成項目寫進「給未來 session 的信」交接。

誠實條款：標註這套 harness 的極限。拆解、驗證、多模型評審補得了執行品質；模糊題與品味判斷補不了，要寫明遇到時怎麼辦（升級到 Opus 4.6/GPT 5.4、外部第二意見、或明說做不到）。subagent 預設不能巢狀委派、instructions 無序合併、memory 的 session scope 會消失——這些平台限制在檔案中要如實寫，不要假裝能做到。不確定的事就查（可用 fetch 工具讀官方文件），查不到就標註，不要編造。

https://github.com/dotnet/skills
https://github.com/DennisWei9898/fable-commander