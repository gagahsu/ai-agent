---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: OpenRouter 介紹與 API Key 申請
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
    OpenRouter 介紹與 API Key 申請
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「一把鑰匙，開啟上百種 AI 模型的大門」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
上一堂課我們把 opencode 裝好了，但它自己不會生成任何內容，因為它只是個「殼」，背後還需要一個真正的 AI 大腦幫忙思考。
今天要介紹的 OpenRouter，就是幫我們接上這個大腦的關鍵服務。
學完這堂課，我們會知道 OpenRouter 是什麼、為什麼要用它，還有怎麼申請自己的 API key。
-->

---
layout: default
---

# Outline

- **1. 什麼是 OpenRouter** — 統一 API 平台的概念
- **2. 註冊帳號** — 申請流程
- **3. 建立 API Key** — 產生金鑰、設定額度上限
- **4. 額度與計費** — Credit 制度介紹

<!--
今天分四段。
先了解 OpenRouter 解決了什麼問題。
再實際手把手註冊帳號、申請 API key。
最後聊聊它的計費方式，讓大家用起來心裡有底。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 什麼是 OpenRouter？
# One API, Hundreds of Models

<!--
先來搞懂 OpenRouter 到底在做什麼。
-->

---

# 什麼是 OpenRouter？

「一個統一的 API 平台，讓你透過同一組金鑰使用上百種 AI 模型」，不用分別申請 OpenAI、Anthropic、Google 各家帳號。

| 沒有 OpenRouter | 有 OpenRouter |
| --- | --- |
| 每家 AI 公司都要單獨申請帳號、金鑰 | 一組 API key 打天下 |
| 換模型要改程式碼串接方式 | 只要改 `model` 參數名稱 |
| 某家服務掛掉就沒得用 | 支援自動 fallback 到其他供應商 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>比喻：</b> OpenRouter 就像「電器轉接頭」，不管你去哪個國家，插頭規格再怎麼不同，透過它都能接上同一顆插座。
</div>

<!--
想像沒有 OpenRouter 的世界：你想試試看 Claude，就要去 Anthropic 申請帳號拿一組金鑰；想玩玩 GPT，又要去 OpenAI 申請另一組；想試 Gemini，還要再去 Google 申請一次。
三家系統、三種串接方式、三份帳單，光管理就很頭痛。
OpenRouter 做的事情，就是站在中間幫你「轉接」，你只要申請 OpenRouter 一組 API key，底下要呼叫哪家的模型，只是改個參數名稱而已，介面統一。
這對我們 opencode 使用者特別方便，因為 opencode 天生就知道怎麼跟 OpenRouter 溝通，接上去就能一次擁有上百種模型可以切換選用。
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

# 註冊與申請 API Key 流程

到 [openrouter.ai](https://openrouter.ai) 註冊，並產生自己的 API key。

| 步驟 | 說明 |
| --- | --- |
| 1. 前往官網註冊 | 可用 Google、GitHub 或 Email 註冊，免信用卡即可開始 |
| 2. 進入 Keys 頁面 | 點右上角個人頭像 → `Keys` |
| 3. 建立金鑰 | 點 `Create Key`，取個名稱（例如 `opencode-key`） |
| 4. 設定額度上限（選用） | 可設定 credit limit，避免超支 |
| 5. 複製並保存 | 金鑰只會完整顯示一次，務必存好 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>金鑰只顯示一次：</b> 離開頁面後就看不到完整金鑰了，記得先存進密碼管理工具或 `.env` 檔案，不要直接寫死在程式碼裡上傳到 GitHub。
</div>

<!--
申請流程其實跟大部分服務差不多，用 Google 或 GitHub 帳號登入最快，而且免費方案不需要信用卡。
登入後，點右上角的頭像，會看到一個 Keys 選項，這裡就是管理所有 API key 的地方。
點 Create Key，取一個好辨識的名字，像是 opencode-key，方便以後知道這把金鑰是給誰用的。
這邊有一個很重要的提醒：這把金鑰只會完整顯示這一次，如果你沒複製就關掉頁面，之後就只能看到局部遮蔽的樣子，只能刪掉重辦。
所以拿到金鑰的當下，先貼到密碼管理工具或是專案的 .env 檔案裡存好，而且千萬不要把這個檔案上傳到公開的 GitHub，等於把家裡鑰匙貼在門口。
-->

---

# 額度與計費模式

OpenRouter 採「Credit（額度）」制度，先儲值再依實際用量扣款。

| 項目 | 說明 |
| --- | --- |
| 計費方式 | 依模型與 token 用量計算，不同模型單價不同 |
| Credit Limit | 可針對單一 API key 設定花費上限 |
| 免費模型 | 部分模型標示 `free`，可零成本先試用 |
| 用量查詢 | 後台可查每把金鑰的即時花費 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>新手建議：</b> 剛開始可以先幫金鑰設定較低的 credit limit（例如 5 美金），並優先選用免費或便宜的模型練習，熟悉之後再逐步調高。
</div>

<!--
OpenRouter 是「先儲值、用多少扣多少」的 credit 制度，不是月租吃到飽。
每個模型的單價不一樣，越強的模型通常越貴，這點跟叫車軟體的車型選擇有點像，選經濟型跟選豪華型價錢不同。
好消息是，有些模型會標示 free，完全不用花錢就能先體驗看看好不好用。
剛開始學習的同學，我會建議幫金鑰設一個比較低的 credit limit，像是 5 塊美金，這樣就算不小心呼叫太頻繁，也不會超支太多，等抓到手感之後再慢慢調高額度。
-->

---
layout: end
---

# 課程結束
### 下一步：把這把 API Key 接進 opencode，正式開始使用！

<!--
今天我們認識了 OpenRouter 這個「統一 API 轉接頭」的角色，也親手申請了帳號跟屬於自己的 API key。
記得把金鑰存好，下一堂課馬上就會用到。
下一堂課，我們要把這把鑰匙接進 opencode，正式讓 AI 開始幫我們寫程式！
-->
