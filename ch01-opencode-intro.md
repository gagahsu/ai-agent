---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: opencode 下載安裝與使用
routeAlias: ch01
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
    opencode 下載、安裝與使用
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「把 AI 工程師請進你的終端機」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
哈囉大家！今天要來認識一個工具，它會住在我們的終端機裡，隨時準備幫我們寫程式、改 bug、解釋程式碼。
它叫做 opencode，是一套開源的 AI coding agent。
你可以把它想成一位「隨傳隨到的工程師夥伴」，不需要開瀏覽器、不需要複製貼上，直接在終端機打字跟它對話就好。
今天學完這堂課，我們就能把 opencode 裝到自己的電腦上，並且學會基本操作。
-->

---
layout: default
---

# Outline

- **1. 什麼是 opencode** — AI coding agent 的定位與特色
- **2. 下載與安裝** — 各平台安裝方式
- **3. 初次設定** — 連接 LLM 供應商、初始化專案
- **4. 基本使用** — TUI 操作、常用指令

<!--
今天的內容分成四段。
第一，先搞懂 opencode 到底是什麼、能做什麼。
第二，實際把它裝進電腦。
第三，第一次啟動時要做的設定。
第四，學會日常操作它的方式。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 什麼是 opencode？
# AI Coding Agent for the Terminal

<!--
先來認識一下今天的主角。
-->

---

# 什麼是 opencode？

「一套開源的 AI coding agent，專門為終端機打造」，可以理解程式碼庫、回答問題、直接幫你改程式。

| 型態 | 說明 |
| --- | --- |
| 終端機介面（TUI） | 主要使用方式，在指令列直接對話 |
| 桌面應用 | 圖形化介面版本 |
| IDE 擴充套件 | 整合進編輯器內使用 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>為什麼要學：</b> opencode 開源、可自由更換 AI 供應商（不綁定單一家），還能接 OpenRouter 使用上百種模型，彈性最高。
</div>

<!--
想像我們平常寫程式遇到卡關，會開瀏覽器搜尋、開 ChatGPT 網頁問問題，再把答案複製貼回編輯器，一來一往很麻煩。
opencode 就是把這位「問答工程師」直接搬進終端機裡，讓它能看到我們專案的檔案、直接幫我們動手改程式。
它最大的特色是「開源」而且「不綁定單一 AI 供應商」，等一下我們會學到怎麼接上 OpenRouter，一次擁有上百種模型可以選。
-->

---

# 下載與安裝

opencode 支援多種安裝方式，挑一種適合自己系統的即可。

| 方式 | 指令 |
| --- | --- |
| 官方安裝腳本 | `curl -fsSL https://opencode.ai/install \| bash` |
| npm | `npm install -g opencode-ai` |
| Homebrew（macOS） | `brew install anomalyco/tap/opencode` |
| Windows（Chocolatey） | `choco install opencode` |
| Windows（Scoop） | `scoop install opencode` |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>注意：</b> 需要「現代終端機」（如 Windows Terminal、WezTerm、Ghostty），太舊的終端機可能無法正常顯示 TUI 介面。
</div>

<!--
安裝方式很多元，不用糾結選哪個，選一個你平常最熟悉的工具就好。
如果你電腦已經有裝 Node.js，直接 npm install -g 是最快的。
Mac 用戶習慣用 Homebrew 的話也可以直接 brew install。
Windows 的同學，用 Chocolatey 或 Scoop 都行，兩個是類似 npm 但專門管理 Windows 軟體的工具。
提醒一下，這是一個文字介面（TUI）工具，畫面會有顏色、框線，記得用比較新的終端機開啟，舊的 cmd.exe 可能顯示會跑版。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 初次設定
# First Run & Configuration

<!--
安裝好之後，我們來看看第一次啟動要做什麼。
-->

---

# 初次啟動流程

進到專案資料夾後啟動 opencode，並完成基本設定。

| 步驟 | 指令 / 操作 |
| --- | --- |
| 進入專案 | `cd /path/to/project` |
| 啟動 TUI | `opencode` |
| 初始化專案分析 | `/init`（產生 `AGENTS.md`） |
| 連接 AI 供應商 | `/connect` |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>AGENTS.md 是什麼：</b> opencode 分析完專案後產生的說明檔，讓 AI 之後理解這個專案的架構與慣例，等於幫它寫一份「新人交接手冊」。
</div>

<!--
想使用 opencode，第一步永遠是先 cd 進入你要處理的專案資料夾，這點跟一般終端機工具一樣。
接著打 opencode，會開啟一個文字介面的對話視窗。
第一次使用建議先打 /init，opencode 會掃描整個專案，自動生成一份 AGENTS.md，就像新人到職前，前輩幫忙寫的「專案交接筆記」，讓 AI 以後回答問題時更懂這個專案的來龍去脈。
最後用 /connect 來設定要用哪一家 AI 模型，這步下一章會細講怎麼接 OpenRouter。
-->

---

# 初次啟動 — 範例

```bash
# 進入你的專案資料夾
cd ~/projects/my-app

# 啟動 opencode（第一次會問你要不要 /init）
opencode

# 在 opencode 對話框內輸入
/init

# 連接 AI 供應商
/connect
```

<!--
我們實際操作一次。
先 cd 到專案，再打 opencode 開啟介面。
opencode 開啟後就是一個輸入框，我們直接打斜線指令，例如 /init。
執行完後，专案資料夾裡就會多一份 AGENTS.md，之後每次跟 AI 對話，它都會先讀過這份筆記。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 基本使用
# Daily Usage

<!--
設定好之後，來看看日常怎麼跟 opencode 互動。
-->

---

# 常用斜線指令

在 opencode 的對話框裡，用 `/` 開頭可以呼叫內建功能。

| 指令 | 說明 |
| --- | --- |
| `/init` | 分析專案並產生 AGENTS.md |
| `/connect` | 設定 AI 供應商與 API key |
| `/models` | 切換使用的模型 |
| `/undo` / `/redo` | 復原／重做剛才的變更 |
| `/share` | 產生可分享的對話連結 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>Plan Mode：</b> 按 Tab 鍵切換「規劃模式」，AI 只會先描述打算怎麼改，不會真的動手改檔案，適合先確認方向再執行。
</div>

<!--
opencode 的操作邏輯很像我們平常用的聊天機器人，只是多了這些「斜線指令」當捷徑。
最常用的除了 /init、/connect，還有 /undo 跟 /redo，AI 改壞你的程式碼時，一鍵復原不用自己 git checkout。
另外提醒一個很實用的技巧：按 Tab 鍵可以切到「Plan Mode」，這時候 AI 只會跟你講解它打算怎麼做，不會真的動手改檔案，等於先看藍圖再蓋房子，避免它亂改一通。
還可以用 @ 符號快速搜尋並引用專案裡的檔案，跟 AI 說「幫我看 @app.js」就會直接帶入那個檔案內容。
-->

---

# CLI 指令（非互動模式）

opencode 除了互動式 TUI，也能用一般指令列方式執行單次任務。

```bash
# 直接下一個任務，不進入互動介面
opencode run "解釋這個專案的資料夾結構"

# 查看目前已設定的 AI 供應商
opencode auth list

# 查看有哪些可用的模型
opencode models

# 啟動無介面的 HTTP 伺服器
opencode serve
```

<!--
除了打開黑底介面慢慢對話，opencode 也支援「一次性任務」的用法，很適合寫在腳本或 CI 流程裡。
opencode run 後面直接接你的問題，它執行完就會印出結果，不會停留在互動畫面。
opencode auth list 可以確認目前有哪些供應商的金鑰已經設定好了。
opencode serve 則是把 opencode 變成一個背景服務，讓其他工具透過 API 呼叫它，這個進階用法我們先知道就好。
-->

---
layout: end
---

# 課程結束
### 下一步：認識 OpenRouter，接上上百種 AI 模型！

<!--
今天我們認識了 opencode 是什麼、怎麼安裝、怎麼做初次設定，也學會了幾個最常用的斜線指令。
大家可以先在自己電腦上把 opencode 裝起來，試著對它下一個簡單的任務看看。
下一堂課，我們要來認識 OpenRouter，學會怎麼申請 API key，讓 opencode 能接上更多、更便宜的 AI 模型！
-->
