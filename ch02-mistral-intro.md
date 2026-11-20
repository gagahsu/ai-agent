---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: Mistral 介紹與 API Key 申請
routeAlias: ch02
style: |
  .slidev-layout p,
  .slidev-layout li,
  .slidev-layout td,
  .slidev-layout th,
  .slidev-layout div {
    font-size: max(16px, 1em);
  }
  table {
    width: 100%;
    margin: 1rem 0;
    border-collapse: collapse;
  }
  th, td {
    padding: 8px !important;
    border: 1px solid #e2e8f0 !important;
  }
  .index-table td {
    text-align: center;
    font-family: monospace;
  }
---

<div class="flex flex-col justify-center items-center h-full" style="background: #ffffff;">
  <p style="color: #5eada0; font-size: 1rem; font-weight: 600; letter-spacing: 0.2em; text-transform: uppercase; margin-bottom: 1.2rem;">
    AI Coding Agent
  </p>
  <h1 style="color: #1a5c5c; font-size: 3.2rem; font-weight: 900; line-height: 1.15; margin-bottom: 1.5rem;">
    Mistral 介紹與 API Key 申請
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「幫 opencode 找一顆會寫程式的 AI 大腦」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
上一堂課我們把 opencode 裝好了，但它自己不會生成任何內容，因為它只是個「殼」，背後還需要一個真正的 AI 大腦幫忙思考。
這門課我們選用的大腦，是法國 AI 公司 Mistral 提供的模型。
學完這堂課，我們會知道 Mistral 是什麼、它有哪些適合寫程式的模型，還有怎麼申請自己的 API key。
-->

---
layout: default
---

# Outline

- **1. 什麼是 Mistral** — 公司與模型介紹
- **2. 註冊帳號** — 建立帳號與選擇方案
- **3. 建立 API Key** — 產生金鑰、安全保存
- **4. 方案與計費** — 免費方案的限制與注意事項

<!--
今天分四段。
先認識 Mistral 這家公司，以及它有哪些模型。
再實際手把手註冊帳號、申請 API key。
最後聊聊免費方案跟付費方案的差別，讓大家用起來心裡有底。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 什麼是 Mistral？
# AI Models from Europe

<!--
先來認識 Mistral。
-->

---

# 什麼是 Mistral？

「一家來自法國的 AI 公司」，提供聊天服務 Le Chat，也透過 API 讓開發者把它的模型接進自己的工具，例如 opencode。

| 產品 | 說明 | 這門課會用到嗎 |
| --- | --- | --- |
| Le Chat | 網頁 / App 版聊天機器人，類似 ChatGPT | ❌ |
| API（La Plateforme） | 讓程式呼叫 Mistral 模型的服務，要用 API key | ✅ |
| Mistral Vibe | Mistral 自己的終端機 coding agent | ❌（我們用 opencode） |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>為什麼選 Mistral：</b> 有專門為寫程式設計的模型（Devstral），價格相對便宜，還有免費方案可以先試用。opencode 也內建支援 Mistral，不需要額外的轉接服務。
</div>

<!--
Mistral 是一家法國的 AI 公司，大家可能聽過它的聊天服務 Le Chat，用起來就像 ChatGPT。
不過我們這門課要用的不是聊天網頁，而是它的 API。API 就是讓程式呼叫 AI 模型的管道，opencode 透過 API 把我們的問題送給 Mistral，再把回答拿回來。
Mistral 自己也有一個終端機工具叫 Mistral Vibe，功能跟 opencode 類似。我們選 opencode 的原因是它不綁定單一家，以後想換別家模型也不用重學工具。
選 Mistral 當大腦有幾個好處：有專門寫程式的模型、價格便宜、有免費方案，而且 opencode 內建就支援它。
-->

---

# 適合寫程式的 Mistral 模型

Mistral 有很多模型，這門課主要會用到這幾個。

| 模型 ID | 特色 | 適合用來 |
| --- | --- | --- |
| `devstral-latest` | 專門為 coding agent 設計 | **主力**：讀專案、改程式、執行任務 |
| `mistral-medium-latest` | 通用模型，速度與能力平衡 | 解釋概念、一般問答 |
| `mistral-large-latest` | Mistral 能力最強的通用模型 | 複雜的規劃與推理 |
| `codestral-latest` | 程式碼補全專用 | 編輯器自動補全（opencode 較少用到） |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>模型名稱會更新：</b> 結尾是 <code>-latest</code> 的 ID 會自動指向最新版本。Mistral 偶爾會淘汰舊模型，以官方文件的模型清單為準。
</div>

<!--
Mistral 的模型很多，這門課主要記住這四個就好。
devstral-latest 是我們的主力，它是專門為 coding agent 設計的，很擅長讀整個專案、改程式、照步驟完成任務。
mistral-medium 跟 mistral-large 是通用模型，medium 比較快、比較省，large 能力最強，適合比較複雜的規劃。
codestral 是給編輯器做自動補全用的，我們在 opencode 裡比較少用到。
提醒大家，模型名稱後面是 -latest 的，會自動指向最新版本，這樣比較不用擔心舊模型被淘汰。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 註冊帳號與建立 API Key
# Sign Up & Get Your Key

<!--
了解完概念，我們實際動手申請帳號跟金鑰。
-->

---

# 註冊帳號與選擇方案

到 [console.mistral.ai](https://console.mistral.ai) 註冊，並選擇方案。

| 步驟 | 說明 |
| --- | --- |
| 1. 前往主控台註冊 | 可用 Google 帳號或 Email 註冊 |
| 2. 建立工作區（Workspace） | 第一次登入會請你建立工作區，取個名字即可 |
| 3. 選擇方案 | **Experiment**（免費）或 **Scale**（付費、依用量計費） |
| 4. 驗證手機 | 免費方案需要驗證手機號碼 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>新手建議：</b> 先選免費的 Experiment 方案練習，熟悉之後如果覺得額度不夠，再升級成 Scale。
</div>

<!--
申請流程跟大部分服務差不多，到 console.mistral.ai 註冊，用 Google 帳號登入最快。
第一次登入會請你建立一個工作區，就當作是你的帳號空間，取個名字就好。
接著要選方案。Experiment 是免費方案，Scale 是付費方案，用多少付多少。
選免費方案的話，需要驗證手機號碼，這是為了防止有人大量申請免費帳號。
剛開始學習，建議先用免費方案，等真的覺得不夠用再升級。
-->

---

# 建立 API Key

| 步驟 | 說明 |
| --- | --- |
| 1. 進入 API Keys 頁面 | 在主控台左側選單找到 `API Keys` |
| 2. 建立金鑰 | 點 `Create new key`，取個名稱（例如 `opencode-key`） |
| 3. 設定到期日（選用） | 可設定金鑰到期時間，到期後自動失效 |
| 4. 複製並保存 | 金鑰只會完整顯示一次，務必存好 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>金鑰只顯示一次：</b> 離開頁面後就看不到完整金鑰了，記得先存進密碼管理工具。千萬不要寫死在程式碼裡上傳到 GitHub。新金鑰可能要等幾分鐘才會生效。
</div>

<!--
帳號建好之後，到左側選單找到 API Keys，點 Create new key。
取一個好辨識的名字，像是 opencode-key，方便以後知道這把金鑰是給誰用的。
你也可以設定到期日，練習用的金鑰設個一兩個月到期，比較安全。
這邊有一個很重要的提醒：這把金鑰只會完整顯示這一次，如果你沒複製就關掉頁面，之後就看不到了，只能刪掉重辦。
所以拿到金鑰的當下，先貼到密碼管理工具存好，而且千萬不要把它放進會上傳到 GitHub 的檔案裡，等於把家裡鑰匙貼在門口。
另外，新建的金鑰有時候要等幾分鐘才會生效，如果馬上用出現錯誤，稍等一下再試。
-->

---

# 方案與計費

| 項目 | Experiment（免費） | Scale（付費） |
| --- | --- | --- |
| 費用 | 免費 | 依模型與 token 用量計費 |
| 使用限制 | 有每分鐘 / 每月的用量上限，較嚴格 | 上限較高 |
| 資料使用 | ⚠️ 你的請求**可能會被用來訓練模型** | 預設不會 |
| 適合 | 學習、練習 | 正式專案、處理重要資料 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>免費方案不要放機密：</b> 用免費方案時，不要讓 AI 讀取公司程式碼、個人資料或任何機密檔案。確切的用量上限請到主控台的 <code>Limits</code> 頁面查看，用量可在 <code>Usage</code> 頁面查詢。
</div>

<!--
最後來看兩個方案的差別。
免費方案當然最吸引人，但它有用量上限，比付費方案嚴格很多。如果你發現 AI 常常回覆說「請求太多」，通常就是碰到上限了，等一下再試就好。
另外有一個很重要的點：免費方案的請求，可能會被 Mistral 拿去訓練模型。所以用免費方案的時候，千萬不要讓 AI 讀取公司的程式碼或任何機密資料。
付費方案依照用量計費，越強的模型通常越貴。
確切的上限數字 Mistral 會調整，請到主控台的 Limits 頁面看；花了多少，可以在 Usage 頁面查。
-->

---
layout: end
---

# 課程結束
### 下一步：把這把 API Key 接進 opencode，正式開始使用！

<!--
今天我們認識了 Mistral 這家公司跟它適合寫程式的模型，也親手申請了帳號跟屬於自己的 API key。
記得把金鑰存好，下一堂課馬上就會用到。
下一堂課，我們要把這把鑰匙接進 opencode，正式讓 AI 開始幫我們寫程式！
-->
