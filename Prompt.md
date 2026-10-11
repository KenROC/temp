# 審視的 prompt

用旁觀者角度回看我剛剛這段工作。不要客套、不要給我面子，重點全放在我可以更好的地方，照這個結構講：
工作特徵：這段我有哪些習慣／慣性（好壞都講）
盲點：至少 3 個我可能沒意識到的，每個附上對話中的具體例子（第幾步、哪句話）
最該改的一件事：只能改一個的話，哪個影響最大
下次的具體做法：給我一個下次能直接照做的調整
請區分『你親眼看到的事實』和『你的推論』，不確定的標出來。
---
# 切換 session 的 prompt

請建立一份我可以貼到新聊天中的交接摘要。請包含：我們想完成什麼、目前已做出的關鍵決定、有哪些重要資訊是你如果用猜的可能會猜錯的，以及下一步該做什麼。請保持精簡，讓人能在一分鐘內讀完。
---
# 自我迭代

(原始版本)實作之後檢驗功能和設計，持續迭代並檢驗，直到達到可上線水準。
(修改後版本)實作完成後，必須完成以下檢查並通過才可停止：
1. 功能：所有主要使用者路徑可正常執行，無明顯錯誤。
2. 測試：核心功能有對應的單元測試或整合測試，且全部通過。
3. 設計：介面/架構符合既定規範（命名、結構、錯誤處理），無明顯技術債。
4. 迴歸：修改後既有功能未壞掉。
若任一項未通過，修正後重新檢驗，最多迭代 3 輪；超過則輸出目前狀態與未解決問題清單後停止。
---
# 簡單暴力的直接叫他拆

把這個功能拆成獨立任務，複雜的給 Terra High，清晰可驗證的給 Luna Max，最後你自己做最終 review。
---
# 解釋概念

請以接近 80% ASD-STE100 規範的方式解釋這個概念。
---
# 整理長文重點

請用 Hierarchical Summarization + Multi-Pass Reading Notes + Content Decomposition Framework
幫我整理這篇長文，並產生可直接放入筆記軟體的重點筆記。
---
# 圖解長文重點

請先用 Mind Map Generation + Argument Map + Flow Diagram Extraction
圖解這篇長文的結構，再用 Multi-Pass Reading Notes 產生重點筆記。

請用 Concept Map Extraction + Knowledge Graph Construction + Hierarchical Summarization
先圖解這篇長文，再產生可直接放入筆記軟體的重點摘要。

---
# 教科書抓重點

請用 Instructional Design Analysis + Chapter Concept Map + Didactic Reduction
幫我拆解這個教科書章節，並產生可直接放入筆記軟體的重點筆記。

請用 Curriculum Mapping + PCK Extraction + Advance Organizer Extraction
整理這個教科書章節的圖解與重點。
---
# 閱讀論文抓重點

請用 Structured Paper Summary (SPS) + Keshav Three-Pass Method + Critical Appraisal Checklist
幫我產生論文閱讀重點筆記。

請依照 MIT/Harvard Paper Dissection Template + RRL Matrix + IMRaD Extraction
產生可直接放入筆記軟體的論文閱讀重點筆記。
