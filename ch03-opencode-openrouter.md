---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: opencode + OpenRouter 的使用
routeAlias: ch03
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
    opencode + OpenRouter 的使用
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「把鑰匙插進車子，正式發動 AI 引擎」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
回顧一下前兩堂課：我們裝好了 opencode 這個殼，也申請好了 OpenRouter 這把萬用鑰匙。
今天要做的事情，就是把這兩個東西正式接在一起，讓 opencode 真正擁有 AI 大腦可以幹活。
學完這堂課，我們就能用 opencode 搭配 OpenRouter，自由切換各種模型來寫程式。
-->

---
layout: default
---

# Outline

- **1. 回顧** — opencode 與 OpenRouter 各自扮演的角色
- **2. 連接 OpenRouter** — `/connect` 圖形化設定
- **3. 設定檔進階配置** — `opencode.json` 手動設定
- **4. 選擇與切換模型** — `/models` 實際操作

<!--
今天四段：先快速回顧兩者關係，接著實際用 /connect 接上金鑰，再看進階的設定檔寫法，最後練習怎麼選模型、切換模型。
-->

---

# 回顧：opencode 與 OpenRouter 的關係

「opencode 是操作介面，OpenRouter 是背後提供 AI 能力的供應商之一」，兩者是搭配關係，不是同一個東西。

| 角色 | 負責的事 |
| --- | --- |
| opencode | 終端機介面、讀寫檔案、下指令執行任務 |
| OpenRouter | 提供實際思考與生成內容的 AI 模型 |
| API Key | 兩者之間的通行證 |

<!--
再複習一次分工：opencode 像是駕駛座跟方向盤，負責跟我們互動、幫我們操作檔案；OpenRouter 則像是引擎，真正負責「思考」的是它背後串接的各家 AI 模型。
而 API key，就是連接駕駛座跟引擎中間那條油管，沒有它，方向盤轉得再用力，車子也不會動。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 連接 OpenRouter
# Connect via /connect

<!--
先用最簡單的方式接上去。
-->

---

# 用 /connect 接上 OpenRouter

在 opencode 對話框內輸入 `/connect`，透過互動選單完成設定。

| 步驟 | 操作 |
| --- | --- |
| 1 | 啟動 opencode，輸入 `/connect` |
| 2 | 在供應商清單中搜尋並選擇 `OpenRouter` |
| 3 | 貼上上一堂課申請的 API key |
| 4 | 完成後輸入 `/models` 挑選要使用的模型 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>金鑰存放位置：</b> 設定完成後，金鑰會存在 <code>~/.local/share/opencode/auth.json</code>，之後開新專案不用重複設定。
</div>

<!--
最簡單的接法就是 /connect，這是一個互動式的精靈，會列出所有支援的供應商讓你搜尋。
打 OpenRouter，選它，接著把上一堂課複製下來的 API key 貼進去按下 Enter，就完成連接了。
連接完的金鑰會存在電腦裡一個固定的設定檔裡，所以只要設定過一次，之後不管開哪個專案的 opencode，都能直接用，不用每個專案重新貼一次金鑰。
接完之後記得輸入 /models，才能挑選你想用哪個模型來對話。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 設定檔進階配置
# opencode.json

<!--
除了互動式設定，我們也能用設定檔手動控制細節。
-->

---

# opencode.json 基本結構

在專案根目錄建立 `opencode.json`，可精確控制要用哪些 OpenRouter 模型。

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openrouter": {
      "models": {
        "anthropic/claude-sonnet-4.5": {}
      }
    }
  }
}
```

<!--
互動式的 /connect 適合快速上手，但如果你想固定某個專案只用特定模型，或是團隊共用同一份設定，就適合寫成 opencode.json 存進版本控制。
這份設定檔的重點是 provider 底下的 openrouter 區塊，裡面列出你要開放使用的模型代號。
這個代號就是 OpenRouter 官網上該模型的 slug，複製貼上即可，不用自己亂猜。
-->

---

# 進階：指定路由偏好

進一步可以指定某個模型優先走哪個底層供應商，避免不穩定。

```json
{
  "provider": {
    "openrouter": {
      "models": {
        "moonshotai/kimi-k2": {
          "options": {
            "provider": {
              "order": ["baseten"],
              "allow_fallbacks": false
            }
          }
        }
      }
    }
  }
}
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>注意：</b> 同一個模型在 OpenRouter 上可能有多家底層供應商同時提供，速度與價格會有差異，不指定的話系統會自動選擇。
</div>

<!--
同一個模型名稱，在 OpenRouter 底層可能同時有好幾家公司在提供服務，速度、價格會有落差。
如果你發現某家常常回應比較慢或不穩，可以像範例這樣，用 order 指定優先走哪一家，並且用 allow_fallbacks: false 表示「不指定的這家不穩，我寧願失敗也不要換別家」。
這是比較進階的用法，一般練習階段用預設值即可，不用急著調這些細節。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 選擇與切換模型
# Pick Your Model

<!--
最後來看實際怎麼在多個模型之間切換。
-->

---

# 切換模型與開始對話

模型設定好之後，用 `/models` 切換，然後直接打字對話即可開始使用。

```bash
# 啟動 opencode
opencode

# 對話框內查看目前可用模型
/models

# 選定模型後，直接輸入任務
幫我在 src/utils 底下新增一個 formatDate 函式，
輸入 Date 物件，回傳 yyyy-mm-dd 格式字串
```

<!--
一切設定就緒後，日常使用就很單純：啟動 opencode，需要換模型時打 /models 選一下，接著就直接像跟同事講話一樣，描述你想要它做的任務。
它會讀懂你的專案結構，找到合適的檔案位置，甚至直接幫你把程式碼寫好、存檔。
提醒大家，任務描述得越具體，AI 產出的結果越準確，例如像範例這樣講清楚「放在哪個資料夾」「輸入輸出格式是什麼」，效果會比只丟一句「幫我寫個日期函式」好很多。
-->

---
layout: end
---

# 課程結束
### 下一步：學會安裝與使用 Agent Skills，讓 opencode 更懂你的專案！

<!--
今天我們把 opencode 跟 OpenRouter 正式接軌，學會用 /connect 快速設定，也看了 opencode.json 的進階寫法，最後練習了怎麼切換模型、下達任務。
大家可以試著換兩三個不同的模型，感受一下回答風格跟速度的差異。
下一堂課，我們要進一步認識 Agent Skills，讓 opencode 學會更多客製化的專業技能！
-->
