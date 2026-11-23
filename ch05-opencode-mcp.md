---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: opencode MCP 介紹、安裝與使用
routeAlias: ch05
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
    MCP 介紹、安裝與使用
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「幫 AI 的雙手接上外部世界的工具箱」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
前一堂課我們教會了 opencode 記住固定流程（Skill），但有些事情光靠「記住怎麼做」還不夠，因為它需要真正連到外部系統才能動手，例如查詢市場熱度、查廣告投放狀況。
今天要介紹的 MCP，就是讓 opencode 的雙手能伸到專案之外，接上這些外部服務的接口。
這堂課我們會用一個實戰情境貫穿全程：用免費 API 做「AI 選品」，判斷一個商品點子值不值得做。
學完這堂課，我們會知道 MCP 是什麼、怎麼設定、怎麼安全地使用它。
-->

---
layout: default
---

# Outline

- **0. 情境：AI 選品** — 這堂課要解決的實際問題
- **1. 什麼是 MCP** — Model Context Protocol 的概念
- **2. 設定 MCP Server** — local 與 remote 兩種類型
- **3. 安裝與驗證** — `opencode mcp add`、OAuth 授權
- **4. 使用與權限管理** — 呼叫工具、控制存取範圍

<!--
今天先講一個情境，再四段：先理解 MCP 概念，再學設定檔怎麼寫，接著練習安裝跟驗證流程，最後看怎麼在對話中使用、以及怎麼管控權限避免濫用。
全程我們都會拿「選品」這件事當例子，讓大家看到 MCP 到底解決了什麼實際問題。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 0. 情境：AI 選品
# Product Research with Free APIs

<!--
先講清楚今天要解決的問題。
-->

---

# 情境：這個商品值得做嗎？

想像我們在做電商，手上有一個商品點子，想知道「現在做這個，是不是好時機」。單靠 AI 自己的知識庫沒辦法回答，因為市場熱度是即時變動的資料。

| 想確認的問題 | 需要的即時資料 |
| --- | --- |
| 大家最近有在搜這個關鍵字嗎？ | Google 搜尋熱度趨勢 |
| 競品有沒有在大量投廣告？ | 廣告投放資料 |
| 網路上大家怎麼討論這個東西？ | 社群討論聲量 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>三個免費資料源：</b> 這堂課會用 <b>Google Trends</b>（搜尋熱度）、<b>Meta Ad Library</b>（廣告熱度）、<b>Reddit</b>（討論聲量）三個免費 API，示範怎麼接成 MCP 給 opencode 用。
</div>

<!--
選品最怕的就是憑感覺，AI 訓練資料是過去的，它不知道「這個月」大家在搜什麼、誰在投廣告。
這正是 MCP 要解決的問題：讓 AI 能連到外部、即時的資料來源，而不是只能靠腦袋裡的舊知識瞎猜。
這堂課我們會用三個完全免費的資料源，一步步接成 MCP，讓 opencode 變成一個會自己查資料、幫你交叉比對的選品助手。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 什麼是 MCP？
# Model Context Protocol

<!--
先來搞懂這個聽起來很技術的名詞。
-->

---

# 什麼是 MCP？

「一套讓 AI 連接外部工具與資料的標準協定」，讓 opencode 除了讀寫本地檔案，還能查詢搜尋熱度、廣告資料、社群討論等外部服務。

| 沒有 MCP | 有 MCP |
| --- | --- |
| AI 只能看得到你電腦裡的檔案 | AI 能即時查 Google Trends、Reddit 等外部資料 |
| 每接一個新服務都要客製化開發 | 用同一套協定接上任何相容服務 |
| 選品判斷全靠 AI 舊知識瞎猜 | AI 能引用當下真實數據下判斷 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>比喻：</b> MCP 就像電腦的 USB 接口，不管你插滑鼠、鍵盤還是隨身碟，規格統一，AI 只要「插上」對應的 MCP server，就能立刻多一項新能力，例如查詢即時市場數據的能力。
</div>

<!--
opencode 本身只知道怎麼讀寫你電腦上的檔案，但如果你想請它「幫我看一下這個關鍵字最近搜尋熱度是漲是跌」，它自己是做不到的，因為那是外部系統的即時資料。
MCP 要解決的正是這件事：它定義了一套統一的溝通規格，讓 AI 跟各種外部工具「插拔」連接，就像 USB 接口一樣，不管接的是滑鼠還是隨身碟，規格都一樣。
只要有人幫某個服務做好了 MCP server（我們接下來要接的 Google Trends、Meta Ad Library、Reddit 都算），我們就能直接接上來用，不用自己重新開發串接程式。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 設定 MCP Server
# Local vs Remote

<!--
MCP server 分成兩種類型，我們分別來看設定寫法，都以選品情境的服務當例子。
-->

---

# Local MCP Server 設定

「本機執行」的 MCP server，透過指令在你電腦上啟動一個小程式提供工具。以查詢 Google Trends 熱度為例（用免費的 `pytrends` 套件包裝）。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "trend-lookup": {
        "type": "local",
        "command": ["python", "trend_mcp_server.py"]
      }
    }
  }
}
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>新版格式：</b> MCP server 要放在 <code>mcp.servers</code> 底下。想暫時關掉某個 server，加上 <code>"disabled": true</code>（舊版的 <code>"enabled"</code> 已不使用）。
</div>

<!--
Local 型的 MCP server，運作方式是 opencode 幫你在背景執行一個指令，這個指令會啟動一支小程式，持續跟 opencode 溝通、提供工具。
這裡的例子是我們自己寫一支 Python 小程式，裡面用免費的 pytrends 套件去抓 Google Trends 的搜尋熱度資料，因為 Google Trends 沒有官方 API，只能靠這種方式包裝成本機服務。
好處是完全免費、不用申請任何金鑰，command 欄位指定怎麼啟動這支程式就好，跟我們平常在終端機下指令的邏輯一樣。
注意新版的格式，server 要放在 mcp 底下的 servers 裡面。網路上的舊教學會直接放在 mcp 底下，還會寫 enabled: true，那是舊版寫法。
-->

---

# trend_mcp_server.py 從哪來？

這支程式不是現成的，我們可以請 opencode 幫忙寫，順便練習前面學到的技巧。

```text
請幫我寫一個 MCP server：trend_mcp_server.py
- 使用 Python 官方的 MCP SDK（mcp 套件）
- 提供一個工具 get_trend(keyword)，用 pytrends 查詢
  這個關鍵字在台灣過去 12 個月的 Google 搜尋熱度
- 回傳每個月的熱度數字
- 寫完告訴我要安裝哪些套件、怎麼測試
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>pytrends 不是官方工具：</b> 它是社群寫的套件，查太頻繁可能被 Google 暫時擋下（出現 429 錯誤），練習時查少量關鍵字就好。
</div>

<!--
大家可能會好奇，trend_mcp_server.py 這支程式要去哪裡下載？答案是：沒有現成的，要自己寫。
不過別擔心，我們手上就有一位 AI 工程師。這段 prompt 用了第 6 章的四要素寫法：目標是 MCP server，背景是 Python 官方的 MCP SDK 跟 pytrends，驗收標準是回傳每個月的熱度。
提醒大家，pytrends 不是 Google 官方的工具，是社群寫的套件，查太頻繁可能會被 Google 暫時擋下來，練習的時候查幾個關鍵字就好。
-->

---

# Remote MCP Server 設定

「架設在雲端」的 MCP server，透過網址連線，不需要本機安裝執行檔。以 Meta Ad Library（查廣告投放熱度）為例。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "meta-ad-library": {
        "type": "remote",
        // 範例網址，請換成實際的 server
        "url": "https://mcp.example.com/meta-ad-library",
        "headers": { "Authorization": "Bearer {env:META_AD_TOKEN}" }
      }
    }
  }
}
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>MCP 會佔用上下文：</b> 接越多 MCP server，AI 能看到的工具越多，但也會吃掉更多對話上下文額度，選品情境建議只接真正會用到的 2–3 個資料源。
</div>

<!--
Remote 型的則不用在本機跑任何程式，直接指定一個網址，opencode 透過網路連過去使用。
這裡的網址 mcp.example.com 只是範例，實際使用時要換成你找到的 MCP server 網址。Meta Ad Library 本身免費、有官方 API，如果社群上已經有人包裝成 MCP server，我們只要填網址跟自己的存取權杖即可；權杖一樣用 {env:META_AD_TOKEN} 語法讀取環境變數，不要寫死在設定檔裡上傳到 GitHub。
提醒一下，我們這堂課想接的是「搜尋熱度、廣告熱度、社群討論」三個資料源，接超過這個數量其實對選品判斷幫助有限，反而讓 AI 要消化的工具說明變多、變笨，抓重點資料源就好。
-->

---

# 三個資料源的限制

接之前先了解每個資料源能做到什麼、做不到什麼，AI 給的結論才不會被誤導。

| 資料源 | 能查到 | 限制 |
| --- | --- | --- |
| Google Trends（pytrends） | 關鍵字的相對搜尋熱度 | 非官方套件，查太頻繁會被擋；只有相對數字，不是實際搜尋量 |
| Meta Ad Library API | 廣告投放資料 | 官方 API 主要涵蓋社會議題、選舉、政治類廣告，以及投放到歐盟的廣告；一般商品廣告建議直接到 Ad Library 網站查看 |
| Reddit API | 貼文與留言討論 | 需要 Reddit 帳號與授權；以英語使用者為主，不代表台灣市場 |

<!--
在讓 AI 下結論之前，我們要先知道每個資料源的限制。
Google Trends 只給相對熱度，是零到一百的比較數字，不是實際搜尋次數。
Meta Ad Library 的官方 API 有一個很大的限制：它主要提供社會議題、選舉、政治類的廣告，以及投放到歐盟的廣告。所以如果你要查一般商品在台灣的廣告，API 很可能查不到，建議直接到 Ad Library 的網站上手動查。
Reddit 是英語社群為主，大家的討論不一定能代表台灣市場。
了解這些限制，你才能判斷 AI 給的結論有幾分可信，這也是用 AI 做判斷時很重要的習慣。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 安裝與驗證
# opencode mcp add

<!--
除了手動寫設定檔，也有指令可以引導完成設定，用 Reddit（查社群討論聲量）示範需要授權的情況。
-->

---

# 用指令快速新增 MCP

不想手寫設定檔，可以用 `opencode mcp add` 直接新增。以接上 Reddit（查社群討論聲量）為例。

```bash
# 新增 remote 類型的 server（網址請換成實際的 server）
opencode mcp add reddit-search --url https://mcp.example.com/reddit

# 新增 local 類型的 server（-- 後面接啟動指令）
opencode mcp add trend-lookup -- python trend_mcp_server.py

# 需要登入授權的 server，執行授權流程
opencode mcp auth reddit-search

# 查看目前接了哪些 server（在 TUI 裡也可以輸入 /mcps）
opencode mcp list
```

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>OAuth 授權：</b> remote server 預設會啟用 OAuth，執行 <code>opencode mcp auth</code> 會跳出瀏覽器讓你登入同意。如果該服務要求事先註冊 app（例如 Reddit），則需要在設定裡填入 <code>client_id</code> 等資訊。
</div>

<!--
不想手動寫設定檔的話，可以用 opencode mcp add。remote 類型用 --url 接網址；local 類型在兩個減號後面接啟動指令。執行後，opencode 會自動幫你寫進設定檔。
需要登入的服務，再執行 opencode mcp auth，它會跳出瀏覽器讓你登入同意。不過要注意，有些服務像 Reddit，會要求你先到它的開發者頁面註冊一個 app，拿到 client id 之後才能授權，這時候就要把 client id 填進設定裡。
最後，opencode mcp list 可以看目前接了哪些 server，在 opencode 的對話介面裡也可以輸入 /mcps 查看。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 4. 使用與權限管理
# Use & Control Access

<!--
三個資料源都接上之後，來看怎麼實際用、怎麼管控。
-->

---

# 在對話中使用 MCP 工具

三個 MCP 接上後會自動出現在 AI 可用的工具清單裡，直接描述選品需求即可，AI 會自己判斷該查哪個工具。

```text
我在考慮賣「露營用可折疊爐具」，
幫我查一下搜尋熱度、有沒有競品在投廣告、
Reddit 上大家怎麼討論，綜合判斷值不值得做
```

| 管理方式 | 設定寫法 |
| --- | --- |
| 暫時關掉整個 server | 在該 server 設定加上 `"disabled": true` |
| 禁止使用某個 server 的工具 | `"permissions": [{ "action": "meta-ad-library_*", "resource": "*", "effect": "deny" }]` |
| 特定 agent 才開放 | 在 `agents.<id>.permissions` 裡把同一條規則設成 `allow` |

<!--
平常我們不用特別去記工具名稱，opencode 對話時會自動判斷需要用哪個 MCP 工具：想查熱度就用 trend-lookup、想查廣告就用 meta-ad-library、想查討論就用 reddit-search，一句話同時觸發三個工具、交叉比對三份數據，這就是 MCP 接上之後的價值。
權限管理的邏輯跟前一堂課的 Skill 一樣，都是用 permissions 規則。MCP 工具的 action 名稱是「server 名稱、底線、工具名稱」，所以 meta-ad-library_* 就代表這個 server 的所有工具。可以先在全域把它設成 deny，再針對特定的 agent 設成 allow，做到「預設收緊、按需開放」的管理方式，避免不必要的工具權限被濫用，例如廣告資料只給行銷用的 agent 開放。
-->

---
layout: default
---

# 練習：用三個免費 API 做一次選品驗證
### 任務說明

在 `opencode.jsonc` 接上 `trend-lookup`（Google Trends）、`meta-ad-library`、`reddit-search` 三個 MCP，針對自己想到的一個商品點子，請 opencode 綜合三份資料給出「值不值得做」的判斷。

---
layout: default
---

# 練習：用三個免費 API 做一次選品驗證
### 解題提示

1. 先請 opencode 寫好 `trend_mcp_server.py`，再接上 `trend-lookup`（local，免驗證），確認能查到搜尋熱度資料
2. 再接 `reddit-search`（remote，需 OAuth），跑一次 `opencode mcp auth reddit-search`
3. 有申請到 Meta 權杖的話，把 `meta-ad-library` 也接上；沒有的話先跳過，或請 AI 改用「手動到 Ad Library 網站查到的結果」當作第三份資料
4. 在對話中具體描述商品點子，讓 AI 自動判斷該呼叫哪些工具，並要求它列出「三份數據分別說了什麼」再下結論

<!--
這個練習讓大家實際體驗一次「幫 AI 接上選品裝備」的完整流程。
提醒大家，先從免驗證的 trend-lookup 開始練習，比較不會卡在授權流程上，等熟悉了流程之後，再挑戰需要 OAuth 或 API 金鑰的服務。
-->

---
layout: end
---

# 課程結束
### 下一步：學會怎麼跟 AI 說話，開始真正的 Vibe Coding！

<!--
今天我們用「AI 選品」這個實戰情境，認識了 MCP：它是什麼、怎麼設定 local 跟 remote 兩種 server、怎麼安裝授權、怎麼在對話中使用並控管權限。
回顧一下整個系列，我們從安裝 opencode 開始，接上 Mistral 拿到 AI 大腦，學會用 Skill 記住固定流程，最後用 MCP 接上外部世界的工具箱，甚至實際做了一次選品驗證。
現在，opencode 已經是一位裝備齊全、隨時待命的 AI 工程師夥伴了。
下一堂課開始，我們要學怎麼跟這位夥伴合作：把需求講清楚、先規劃再動手，正式進入 vibe coding！
-->
