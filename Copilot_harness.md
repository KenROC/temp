這是我這輩子唯一一次使用 Claude Opus 4.8 Max 的機會。這個 session 結束後，日常任務將全程由 VS Code 的 Auto 路由機制長期運作。所以你的任務只有一個：把你的判斷力轉成可長期沿用的制度與檔案，讓之後每一個日常 Auto session 都因此變強。用這個 session 立制度，不要拿去執行日常任務。

作業規則（先讀完再動手）：
1. 自主作業。環境能查的自己查（.github/copilot-instructions.md、.github/agents/、.github/skills/）。開場最多問我五題以內，之後不再停下來等我。
2. 價值排序＋隨做隨寫。先做價值最高的產出，每完成一項立刻寫進檔案再做下一項。這個 session 隨時可能中斷，中斷時已落檔的就是我拿到的全部。
3. 改任何既有檔案前先留備份副本。新內容寫成新檔，.github/copilot-instructions.md 只放精簡路由指向新檔。
4. 你的讀者是未來的 Auto session。寫給它們的規則要具體、可執行、有判準與範例；抽象要求等於沒寫。
5. 【重要防爆限制】日常任務最大的痛點是「Context 太大導致失敗」。所以你設計的所有制度、Custom Agents 與 Skills，必須以「極簡 Context、隔離 Tool Outputs、適時斷點交接」為最高指導原則。

環境事實（寫檔時以此為準，不要憑印象改）：
- 日常調度全程使用 VS Code 內建的 Auto 模型路由，不要在任何 Custom Agent (.agent.md) 寫死 `model` 欄位。
- 節省 Context 的官方解法：長內容轉為 Agent Skills（漸進式載入）、大量檔案操作交給 Subagent 隔離（不污染主對話）、長任務主動斷點要求開新 Chat。

交付清單（按價值排序執行）：
A. 快速診斷與瘦身：檢查現有 .github/copilot-instructions.md 與其他設定。若有浪費 context 的過時內容、或該隔離的長內容，直接重構它。落檔為 .github/instructions/00-diagnosis.instructions.md。
B. 重寫核心指令（copilot-instructions.md）：只保留跨語言通則、路由指標。寫入「Auto 路由引導守則」（教未來的 session 在 prompt 開頭加 "COMPLEX:" 或 "ROUTINE:" 以利 Auto 派發資源），以及「Context 紀律」（禁止印出大段原始碼，長檔只列修改點）。
C. 建立低 Context 委派機制（.github/agents/）：
   - 建 worker.agent.md：不寫死 model。工具限縮到讀取、搜尋與編輯。職責是大量讀取、批次改檔。明確指示：「只回報結論、行號與檔案路徑，絕對禁止在對話中印出大段程式碼」。
   - 建 reviewer.agent.md：不寫死 model。唯讀工具。職責是依驗收條件核對，handoffs 指向使用者確認。
   - 在 10-dispatch.instructions.md 寫下守則：主對話禁止親自執行大量掃描，必須用 `agent/runSubagent` 派 worker 執行，以隔離 Context 污染。
D. 判斷力外化（轉為 Agent Skill）：在 .agents/skills/judgment/SKILL.md 建立判斷準則。把只有 Opus 4.8 Max 擅長的判斷，寫成具體 Checklist：何時算完成、何時該停下問人、什麼訊號代表方向錯了該換路、品質底線。放在 Skill 是為了利用漸進式載入省 Context。每條判準附正反例。
E. 防爆版派工 Skills 模板（.agents/skills/）：建立三個日常接任務的 Skill。
   - 建立 delegate-implement/SKILL.md、delegate-refactor/SKILL.md、delegate-review/SKILL.md。
   - 每個 SKILL.md 必須有精簡的 `name` 與 `description`（確保在 8000 字元初始清單內不被截斷）。
   - 【防爆核心】每個模板必須內建「Context Checkpoint」：規定在執行每 3 個檔案修改或發現對話變長時，主動把進度與未完成事項寫入 `handoff.md`，並提示使用者「Context 可能即將耗盡，請開新 Chat 並附上 handoff.md 繼續」，嚴禁硬撐到 OOM。
F. 維護協議（.github/instructions/30-maintenance.instructions.md）：未來的 session 怎麼安全更新這套制度。規定修改 instructions 前必須確認不會大幅增加 Context；踩坑教訓若非全域適用，應寫入特定模組的 Skill，絕不塞進 copilot-instructions.md。
G. 給未來 session 的信（.github/instructions/99-letter.md，不加 .instructions.md 以防自動載入）：三件我沒問、但你認為對這個環境最重要的事，加上你認為這套制度最可能的退化方式與預防法。

收尾（必做，不可省略）：
1. 開一個 fresh-context subagent 對抗審查全部產出：找規則互相打架、YAML/Markdown 格式錯誤、Skill description 太長、防爆機制未落實的漏洞，修完為止。
2. read-back 驗證每個檔案確實落地、內容完整。
3. 給我一頁總結（SUMMARY.md）：改了什麼、為什麼、明天開始日常用 Auto 路由接任務時怎麼操作（如何呼叫防爆 Skills、斷點交接流程）。
4. 如果 context 快用完：立刻停下手上的產出，先完成收尾前三步，把未完成項目寫進「給未來 session 的信」交接。

誠實條款：標註 harness 的極限。拆解、驗證補得了執行品質；模糊題與品味判斷補不了，要寫明遇到時怎麼辦（外部第二意見、或明說做不到）。不確定的事就查，查不到就標註，不要編造。

https://github.com/dotnet/skills
https://github.com/DennisWei9898/fable-commander