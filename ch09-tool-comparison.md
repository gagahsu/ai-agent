---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: AI 寫程式工具比較
routeAlias: ch09
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
    Vibe Coding
  </p>
  <h1 style="color: #1a5c5c; font-size: 3.2rem; font-weight: 900; line-height: 1.15; margin-bottom: 1.5rem;">
    AI 寫程式工具比較
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「工具會一直換，會用的技能一直都在」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
到這裡為止，我們都用 opencode 在學 vibe coding。
但市面上 AI 寫程式的工具非常多，大家一定聽過 Cursor、Claude Code、ChatGPT 這些名字。
這堂課要幫大家把這些工具分類，知道它們的差別，以及什麼時候該用哪一個。
-->

---
layout: default
---

# Outline

- **1. 四大類工具** — 聊天、編輯器、終端機 agent、雲端產生器
- **2. 終端機 agent 比較** — opencode 和它的同類
- **3. 什麼時候用哪一個** — 依情境挑工具
- **4. 換工具也帶得走的技能** — 這門課真正學到的東西

<!--
今天分四段。
先把 AI 寫程式工具分成四大類。
再細看跟 opencode 同一類的終端機 agent 有哪些。
接著用情境來判斷什麼時候用哪一種工具。
最後談一個重點：不管換什麼工具，前面學到的技能都帶得走。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 四大類工具
# Four Kinds of Tools

<!--
先來看整體的分類。
-->

---

# AI 寫程式工具的四大類

| 類型 | 代表工具 | 看得到你的檔案？ | 會直接改檔案？ |
| --- | --- | --- | --- |
| 💬 網頁 / App 聊天 | ChatGPT、Claude、Gemini、Le Chat | ❌ 要自己貼 | ❌ 要自己複製貼上 |
| 📝 AI 編輯器 | Cursor、VS Code + GitHub Copilot | ✅ | ✅ |
| 💻 終端機 agent | opencode、Claude Code、Codex CLI、Gemini CLI、Mistral Vibe | ✅ | ✅ 還能執行指令 |
| ☁️ 雲端產生器 | Lovable、Bolt、v0、Replit | ✅（在它的雲端） | ✅ 直接產生整個網站 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>界線越來越模糊：</b> 很多工具同時有多種版本，例如 Claude Code 有終端機、桌面 App、網頁和 IDE 擴充套件；opencode 也有桌面版。分類是幫助理解，不是絕對的。
</div>

<!--
AI 寫程式的工具大概可以分成四類。
第一類是網頁或 App 的聊天機器人，像 ChatGPT、Claude、Gemini。它們看不到你電腦裡的檔案，你要自己複製貼上，但很適合問問題、學概念。
第二類是 AI 編輯器，像 Cursor，或是 VS Code 加上 GitHub Copilot。有圖形介面，看得到你的檔案，也會直接幫你改。
第三類就是我們在用的終端機 agent，除了改檔案，還能幫你執行指令，像是跑測試、啟動伺服器。
第四類是雲端產生器，像 Lovable、Bolt，在瀏覽器裡用一句話就產生整個網站。
要注意的是，現在很多工具同時有好幾種版本，所以這個分類是幫助理解，不是絕對的。
-->

---

# 各類工具的優缺點

| 類型 | 優點 | 缺點 |
| --- | --- | --- |
| 💬 聊天 | 最容易上手、適合學概念和問問題 | 看不到專案，複製貼上容易出錯 |
| 📝 AI 編輯器 | 圖形介面、diff 好閱讀、適合邊看邊改 | 要先熟悉編輯器操作 |
| 💻 終端機 agent | 能讀整個專案、執行指令、自動化程度高 | 要會一點終端機操作 |
| ☁️ 雲端產生器 | 最快看到成品，不用安裝任何東西 | 不容易看懂程式、專案變大後難控制 |

<!--
每一類都有優缺點。
聊天最容易上手，但它看不到你的專案，來回複製貼上很容易出錯。
AI 編輯器有圖形介面，看 diff 很方便，適合想要邊看程式邊改的人。
終端機 agent 能讀整個專案、還能跑指令，自動化程度最高，但需要會一點終端機。
雲端產生器最快看到成品，但它幫你做太多了，你很難知道程式是怎麼運作的，專案一大就很難控制。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 終端機 agent 比較
# Terminal Agents

<!--
接下來細看跟 opencode 同一類的工具。
-->

---

# opencode 和它的同類

| 工具 | 開發者 | 開源？ | 可以用哪些模型 |
| --- | --- | --- | --- |
| **opencode** | 開源專案 | ✅ | 幾乎任何供應商（OpenRouter、Mistral、本機模型…） |
| **Claude Code** | Anthropic | ❌ | Claude 系列 |
| **Codex CLI** | OpenAI | ✅ | GPT 系列 |
| **Gemini CLI** | Google | ✅ | Gemini 系列 |
| **Mistral Vibe** | Mistral AI | ✅ | Devstral 等 Mistral 模型 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>價格與方案變化很快：</b> 各家的訂閱方案、免費額度幾乎每幾個月就會調整，使用前請以官網最新資訊為準。
</div>

<!--
跟 opencode 同類的終端機 agent，主要有這幾個。
Claude Code 是 Anthropic 做的，搭配 Claude 模型。Codex CLI 是 OpenAI 的，Gemini CLI 是 Google 的，Mistral Vibe 是法國 Mistral 公司做的。
這些大廠的工具，通常跟自家模型綁在一起，整合得最好。
opencode 最大的特色是不綁定任何一家，你可以接 OpenRouter、Mistral，甚至在自己電腦上跑的本機模型。
這也是我們這門課選 opencode 的原因：學一套工具，就能試遍各家模型。
最後提醒，各家的價格跟免費額度變化很快，使用前請看官網最新資訊。
-->

---

# 選工具的三個問題

| 問題 | 如果答案是… | 可以考慮 |
| --- | --- | --- |
| 你已經付費訂閱哪一家 AI？ | 有 Claude 訂閱 | Claude Code |
| | 有 ChatGPT 訂閱 | Codex CLI |
| | 有 Mistral API key 或 Le Chat 訂閱 | Mistral Vibe 或 opencode |
| 想不想自由換模型？ | 想多試幾家、比較價格 | opencode + OpenRouter |
| 喜歡終端機還是圖形介面？ | 不習慣終端機 | Cursor、VS Code + Copilot，或各工具的桌面版 |

<!--
選工具其實可以問自己三個問題。
第一，你已經付費訂閱哪一家？如果你已經有 Claude 訂閱，用 Claude Code 最划算；有 ChatGPT 訂閱，就用 Codex。
第二，你想不想自由換模型？想多試幾家、比較價格的話，opencode 加 OpenRouter 最有彈性。
第三，你喜歡終端機還是圖形介面？如果真的不習慣終端機，可以用 Cursor 或 VS Code，或是這些工具的桌面版。
沒有哪一個是最好的，只有最適合你現在狀況的。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 什麼時候用哪一個
# Pick by Situation

<!--
接下來用實際情境來看。
-->

---

# 依情境挑工具

| 情境 | 建議 | 原因 |
| --- | --- | --- |
| 搞懂一個概念（如什麼是 API） | 💬 聊天 | 不需要專案，問答最快 |
| 做個簡單原型給朋友看 | ☁️ 雲端產生器 | 最快看到成果、可分享網址 |
| 在自己的專案加功能、修 bug | 💻 終端機 agent / 📝 編輯器 | 看得到整個專案、能立刻測試 |
| 邊做邊學、看懂每一行 | 📝 編輯器 / 💻 agent（plan） | 可以一邊看程式一邊問 |
| 重複性工作（批次修改） | 💻 終端機 agent | 能執行指令、能自動化 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>可以混著用：</b> 很多人會用聊天學概念、用雲端產生器做出第一版、再下載程式碼用 opencode 繼續改。
</div>

<!--
這張表依照情境整理了建議的工具。
只是想搞懂一個概念，直接用聊天最快。
想很快做個原型給朋友看，雲端產生器最方便，做完還能直接分享網址。
在自己的專案裡加功能、修 bug，就是終端機 agent 或 AI 編輯器的主場。
這些工具不是只能選一個，很多人會混著用：用聊天學概念，用雲端產生器做第一版，再把程式碼下載下來，用 opencode 繼續改。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 4. 換工具也帶得走的技能
# Skills That Transfer

<!--
最後一段，也是我最想跟大家說的。
-->

---

# 這門課真正學到的東西

工具的名字、指令、按鍵一直在變，但這些能力換到任何工具都用得上。

| 你學到的技能 | 在其他工具叫什麼 |
| --- | --- |
| 四要素描述需求、拆小步驟 | 所有工具都適用 |
| 先規劃再動手（plan 模式） | Claude Code、Cursor 等工具也有類似的 Plan 模式 |
| 看懂 diff、檢查修改 | 所有會改檔案的工具都會顯示 diff |
| 錯誤回報、處理鬼打牆 | 所有工具都適用 |
| `AGENTS.md` | Codex 也讀 `AGENTS.md`；Claude Code 用 `CLAUDE.md`；Cursor 用 rules |
| Skill、MCP | 很多工具都支援同樣的 Skill 格式和 MCP 協定 |

<!--
這張表是整門課的總整理。
我們學到的四要素、拆小步驟、先規劃、看 diff、錯誤回報、處理鬼打牆，這些技能換到任何工具都用得上。
就連 AGENTS.md、Skill、MCP 這些設定，很多工具也都支援，或者有很類似的東西。像 Claude Code 用的是 CLAUDE.md，概念完全一樣。
所以大家不用擔心「學了 opencode，以後換工具就白學了」。工具會一直換，但跟 AI 合作的能力是你自己的。
-->

---
layout: default
---

# 練習：同一個任務，兩種工具

### 任務說明

1. 挑一個小任務，例如「做一個顯示今天日期和名言的網頁」
2. 先用 **聊天型 AI**（網頁版）完成：複製程式碼、自己存成檔案、打開測試
3. 再用 **opencode** 完成同一個任務
4. 比較兩次的過程：哪個比較快？哪個比較容易出錯？哪個讓你比較看得懂？

<!--
這個練習要大家親身比較兩種工具。
同一個任務，先用網頁版的聊天 AI 做一次，自己複製貼上程式碼、存檔、測試；再用 opencode 做一次。
做完之後比較看看，你會更清楚每種工具適合的時機。
-->

---
layout: default
---

# 練習：同一個任務，兩種工具

### 解題提示

1. 兩次都用同一份「四要素」需求，比較才公平
2. 用聊天型 AI 時，注意自己複製貼上了幾次、有沒有貼錯
3. 用 opencode 時，記得先 `plan` 再 `build`，並檢查 diff
4. 把比較結果寫成三句話：「我會在＿＿時用聊天型 AI，在＿＿時用 opencode，因為＿＿」

<!--
提示最後一點的三句話，是希望大家整理出自己的判斷標準。
每個人的習慣不一樣，沒有標準答案，重點是你知道自己為什麼這樣選。
-->

---
layout: end
---

# 課程結束
### 恭喜完成 opencode × Vibe Coding 全系列課程！

<!--
今天我們認識了 AI 寫程式工具的四大類、終端機 agent 的同類工具、依照情境挑工具的方法，最後整理了換工具也帶得走的技能。
回顧整個系列：我們裝好 opencode、接上 OpenRouter、學會 Skill 跟 MCP，接著學怎麼跟 AI 說話、怎麼除錯跟學習、怎麼用 AGENTS.md 打造自己的 AI 家教，最後認識了各種工具。
接下來最重要的就是動手做。挑一個自己真的想做的小東西，用這門課學到的方法，一步一步把它做出來吧！
-->
