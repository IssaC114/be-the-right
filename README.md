# 不走冤枉路 (Be the right.) — 基於 RAG 之智慧政務導引平臺

> **「願你不走冤枉路。」**
> 結合 LINE ChatBot 與 RAG 技術，提供即時、正確的公家機關辦事資訊與最佳路徑規劃。

![Project Status](https://img.shields.io/badge/Status-Active-brightgreen) ![n8n](https://img.shields.io/badge/Workflow-n8n-ff6b6b) ![OpenAI](https://img.shields.io/badge/LLM-GPT--4o-412991) ![LINE](https://img.shields.io/badge/Interface-LINE_Bot-06c755)

## 專案簡介

現今網路資訊爆炸，民眾查詢公家機關辦事流程常被過時資訊誤導，導致浪費時間跑錯地點。
**「不走冤枉路」** 是一個基於 **n8n** 自動化流程的智慧導引平臺，透過 **LINE 官方帳號** 提供服務。系統採用 **RAG (檢索增強生成)** 架構，確保回答的正確性，並結合 **Google Maps** 進行多點最佳路徑規劃，解決民眾日常辦事痛點。

## 核心功能

* **智慧問答 (RAG)**：結合向量資料庫與 GPT-4o，精準回答辦事所需文件、地點與營業時間，比一般 AI 模型更可靠，且資料經過人工查證。
* **最佳路徑規劃**：針對需要前往多個機關的民眾 (如戶政事務所、監理站)，自動計算「不走冤枉路」的最佳順序與導航資訊。
* **LINE 即時互動**：無需下載額外 App，像日常聊天一樣簡單易用，降低使用門檻。
* **自動化流程**：使用 n8n 視覺化串接所有服務，實現全自動化回應。

## 系統架構

本專案使用 **n8n** 作為核心後端，採用 **AI Agent** 架構掛載工具 (Tools) 模式運作：

* **使用者介面**：LINE Official Account
* **自動化引擎**：n8n
* **LLM 模型**：OpenAI GPT-4o
* **向量資料庫 (Vector DB)**：Pinecone
* **地圖服務**：Google Maps Directions API

## 快速部署 (Deployment)

本專案提供已配置好的 n8n Workflow JSON 檔案，您可以直接匯入使用。

### 1. 前置需求 (Prerequisites)

在開始之前，請確保您擁有以下服務的帳號與 API Keys：

* **n8n**：已安裝並運行的 n8n 實例 (自架 Docker 或 Cloud 版本)。
* **LINE Developers**：建立 Messaging API Channel，取得 `Channel Access Token` 與 `Channel Secret`。
* **OpenAI**：取得 API Key (需支援 `gpt-4o` 與 `text-embedding-3-large`)。
* **Pinecone**：建立 Index，並準備好已向量化的政務資料。
* **Google Cloud Platform**：啟用 Directions API 並取得 API Key。

### 2. 匯入 Workflow

1.  下載本專案提供的 `.json` 檔案。
2.  開啟您的 n8n 編輯器。
3.  點擊右上角的選單，選擇 **"Import from File"**。
4.  選擇 JSON 檔案匯入。

### 3. 設定憑證 (Credentials)

匯入後，請點擊紅色的節點並設定對應的 Credential：

| 節點名稱 | 需設定的憑證 | 說明 |
| :--- | :--- | :--- |
| **HTTP Request1 (LINE Reply)** | `LINE Access Token` | 用於回覆訊息至 LINE |
| **OpenAI Chat Model** | `OpenAI API` | 用於 GPT-4o 生成回答 |
| **Embeddings OpenAI** | `OpenAI API` | 用於 RAG 向量化查詢 |
| **Pinecone Vector Store** | `Pinecone API` | 設定您的 Index Name |
| **HTTP Request Tool (Maps)** | `Google Maps API Key` | 填入 Query Parameters 中的 `key` |

### 4. 設定 Webhook

1.  開啟 n8n 中的 **Line (Webhook)** 節點。
2.  複製 `Production URL` (或是測試用的 Test URL)。
3.  前往 LINE Developers Console -> Messaging API -> Webhook settings。
4.  貼上 URL 並啟用 "Use webhook"。

## 資料準備 (Data Preparation)

為了確保 RAG 的準確性，資料庫資料需經過人工整理：

1.  收集公家機關 (如區公所、戶政事務所、外交部領事局) 的業務資訊。
2.  整理成表格或結構化文字 (CSV/JSON)。
3.  使用 n8n 或 Python script 將文字轉為 Vector (Embedding)。
4.  存入 Vector Database (Pinecone)。

## 未來展望

* 擴展至全台灣各縣市行政機關。
* 納入學校及更多行政流程。
* 持續優化 AI Agent 的語意理解能力，讓辦事就像聊天一樣簡單。

## 團隊成員

* 周詠熙、陳宜妏、黃湙琁

---
**Be the right.**
