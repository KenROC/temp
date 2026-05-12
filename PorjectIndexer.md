---
name: Project Indexer
description: 自動索引資料夾中的所有專案，產生結構圖、模組關係、依賴關係與工程師可讀的系統理解文件。
---

# 🧩 專案索引器（Project Indexer）

此技能負責在使用者要求「索引專案」、「分析資料夾」、「建立結構圖」等相關任務時啟動。  
目標是協助工程師快速理解整個系統的架構、模組邏輯與專案之間的互動方式。

---

## 🎯 **技能目標**

- 掃描使用者指定的資料夾或整個 Repo
- 自動辨識：
  - 專案（Project）
  - 模組（Module）
  - 套件（Package）
  - 子系統（Subsystem）
- 建立：
  - 專案結構樹（Tree Structure）
  - 專案之間的關聯（Dependencies / Interactions）
  - 程式流程摘要（Flow Overview）
  - 系統架構摘要（Architecture Summary）
- 產生工程師可讀的文件（例如：`README.md`）

---

## 📁 **輸出格式規範**

### 1. 專案結構（Project Structure）
以樹狀結構呈現，例如：

/project-root
/backend
/src
/config
/modules
/frontend
/src
/components
/pages
/docs

---

### 2. 專案關係（Project Relationships）

以簡潔方式描述：

frontend → 呼叫 backend API
backend → 依賴 PostgreSQL
backend → 使用 Redis 做快取

---

### 3. 程式流程摘要（Program Flow Overview）

例如：

使用者登入 → frontend 傳送 JWT → backend 驗證 → DB 查詢 → 回傳結果

Code

---

### 4. 系統架構摘要（Architecture Summary）

例如：

- 前後端分離架構
- Backend 採用分層式架構（Controller / Service / Repository）
- Frontend 採用元件化架構（React / Vue）
- 使用 Docker Compose 管理多服務

---

## 🛠️ **技能行為規則**

### 1. 永遠使用繁體中文  
所有輸出、分析、摘要、結構圖都必須使用繁體中文。

### 2. 不修改程式碼  
此技能只負責「分析」與「產生文件」，不會主動修改任何檔案。

### 3. 若使用者未指定資料夾  
預設分析整個 Repo。

### 4. 若偵測到多個專案  
需自動分類，例如：

- backend（Java / Spring Boot）
- frontend（React / TypeScript）
- worker（Python）
- infra（Terraform / Docker）

### 5. 若偵測到常見框架  
需自動補充工程師需要的理解資訊，例如：

- Spring Boot → 自動辨識 Controller / Service / Repository
- React → 自動辨識 Components / Pages / Hooks
- Node.js → 自動辨識 Routes / Middleware / Services

---

## 📌 **技能觸發條件（Triggers）**

當使用者輸入以下類型指令時啟動：

- 「幫我索引專案」
- 「分析這個資料夾」
- 「產生 README.md」
- 「顯示專案架構」
- 「分析專案之間的關係」
- 「建立系統架構圖」
- 「幫我理解這個 Repo」

---

## 🚀 **技能執行步驟（Procedure）**

1. 掃描資料夾與檔案結構  
2. 自動分類專案與模組  
3. 辨識語言、框架、常見目錄  
4. 建立專案結構樹  
5. 分析專案之間的依賴與互動  
6. 建立程式流程摘要  
7. 建立系統架構摘要  
8. 產生完整的 `README.md`（或回傳給使用者）

---

## 📄 **範例輸出（Example Output）**

專案架構索引結果
一、專案結構
/backend
/src
/modules
/frontend
/src
/components

二、專案關係
frontend → backend（REST API）
backend → PostgreSQL
backend → Redis（快取）

三、程式流程摘要
登入流程：Frontend → Backend → DB → 回傳 Token

四、系統架構摘要
前後端分離
Backend 採用分層式架構
Frontend 採用元件化架構

---

## 🔚 **技能限制**

- 不會推測不存在的專案
- 不會修改任何檔案（除非使用者明確要求）
- 不會執行程式碼

---