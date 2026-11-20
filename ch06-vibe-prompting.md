---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: 怎麼跟 AI 說話
routeAlias: ch06
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
    怎麼跟 AI 說話
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「AI 很會寫程式，但它不會讀心術」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
前面幾堂課，我們把 opencode 裝好、接上模型，工具已經準備好了。
但很多同學第一次用會發現：AI 做出來的東西常常不是自己要的。
問題通常不在 AI 不夠聰明，而是我們沒有把需求講清楚。
這堂課就是要練習 vibe coding 最核心的技能：怎麼把腦袋裡的想法，講成 AI 聽得懂、做得對的任務。
-->

---
layout: default
---

# Outline

- **1. 把需求講清楚** — 好任務的四個要素
- **2. 一次只做一件事** — 把大目標拆成小步驟
- **3. 先規劃再動手** — Plan 模式的工作流程
- **4. 讀懂 AI 的修改** — 看懂 diff，不盲目接受

<!--
今天分四段。
先學怎麼描述一個好任務。
再學怎麼把一個大目標拆小，讓 AI 每一步都做得穩。
接著學 Plan 模式，讓 AI 先講計畫、我們確認後再動手。
最後學怎麼檢查 AI 改了什麼，這是保護自己專案最重要的習慣。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 把需求講清楚
# Clear Instructions

<!--
先從最基本的開始：一個好的任務描述長什麼樣子。
-->

---

# 好任務的四個要素

把 AI 當成「很厲害但第一天上班的新同事」，它不知道你腦中的畫面，你要主動交代。

| 要素 | 要回答的問題 | 範例 |
| --- | --- | --- |
| 🎯 目標 | 要做出什麼？ | 做一個待辦清單網頁 |
| 📂 背景 | 在哪裡做？用什麼技術？ | 純 HTML + CSS + JavaScript，寫在 `index.html` |
| 🚧 限制 | 什麼不能做、不能動？ | 不要安裝任何套件、不要改 `style.css` |
| ✅ 驗收標準 | 怎樣才算完成？ | 能新增、刪除項目，重新整理後資料還在 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>小技巧：</b> 驗收標準最常被忘記，但它最重要。沒有驗收標準，AI 不知道什麼時候該停，你也不知道它到底做對了沒。
</div>

<!--
想像你請一位新同事幫忙，只說「幫我做個網頁」，他一定會一直問：什麼網頁？用什麼做？做到什麼程度？
AI 也一樣，差別是它不會問，它會自己猜，猜錯了你就要花時間來回修。
所以交代任務時，記得這四件事：目標、背景、限制、驗收標準。
其中驗收標準最常被忘記。像「重新整理後資料還在」這句話，就決定了 AI 會不會幫你把資料存進瀏覽器。少了這句，它很可能只做出一個重新整理就清空的版本。
-->

---

# 爛 prompt vs 好 prompt

同一件事，講法不同，結果差很多。

❌ **模糊版**：AI 可能會用 React 建一整個專案、裝一堆套件，做出你看不懂的架構。

```text
幫我做一個待辦清單
```

✅ **清楚版**：每一行都在幫 AI 縮小範圍。

```text
在 index.html 做一個待辦清單：只用 HTML、CSS、原生 JavaScript。
不要安裝任何套件。
功能：可以新增、刪除、打勾完成。用 localStorage 存資料，重新整理後還在。
做完告訴我怎麼用瀏覽器打開測試。
```

<!--
左邊這句話，AI 有一百種做法，它可能幫你開一個 React 專案，裝十幾個套件，對初學者來說根本看不懂。
右邊這句雖然長一點，但每一行都在幫 AI 縮小範圍：用什麼技術、要什麼功能、資料怎麼存、什麼不能做。
最後一行「做完告訴我怎麼測試」也很實用，讓 AI 主動交代驗收方法，你就知道下一步要做什麼。
多花三十秒把需求寫清楚，可以省下三十分鐘的來回修改。
-->

---

# 給 AI 足夠的上下文

AI 只看得到你給它的東西，該給的資訊不要省。

| 情境 | 怎麼給 |
| --- | --- |
| 要它看某個檔案 | 用 `@` 引用，例如 `@index.html`，也可以指定行數 `@app.js#20-45` |
| 程式出錯了 | 把**完整**錯誤訊息貼上，不要只說「壞掉了」 |
| 畫面跑版 | 截圖貼上，並描述「預期長怎樣、實際長怎樣」 |
| 想要某種風格 | 給一個參考範例或網址，比形容詞有用 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>不要假設 AI 記得：</b> 開新的對話（session）之後，AI 不會記得上一次聊過什麼。重要的規則要寫進 <code>AGENTS.md</code>（後面的章節會教）。
</div>

<!--
AI 再厲害，也只能根據你給它的資訊判斷。
在 opencode 裡打 @ 就可以搜尋並引用專案檔案，還可以指定行數，像 @app.js#20-45，讓它只專心看那一段。
程式出錯時，最常見的錯誤就是只跟 AI 說「壞掉了」。請把完整的錯誤訊息複製貼上，後面除錯那一章會教怎麼看錯誤訊息。
另外提醒，開新對話後 AI 是不會記得之前聊過的內容的，如果有每次都要遵守的規則，就要寫進 AGENTS.md。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 一次只做一件事
# Small Steps

<!--
接下來談一個新手最常踩的坑：一次叫 AI 做太多事。
-->

---

# 把大目標拆成小步驟

一次給太多任務，AI 容易漏東漏西，出錯時你也不知道是哪一步壞的。

| 步驟 | 任務 | 完成後先確認 |
| --- | --- | --- |
| 1 | 做出靜態畫面：輸入框、按鈕、清單 | 瀏覽器打開看得到畫面 |
| 2 | 按按鈕可以新增項目 | 能新增 |
| 3 | 每個項目可以刪除、打勾 | 能刪除、能打勾 |
| 4 | 用 localStorage 存資料 | 重新整理後還在 |
| 5 | 美化樣式 | 看起來舒服 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>每完成一步就存檔：</b> 確認這一步沒問題，就用 Git commit 存一個存檔點，下一步改壞了可以退回來。
</div>

<!--
蓋房子不會一次把地基、牆壁、屋頂、裝潢全部同時做，寫程式也一樣。
如果一次跟 AI 說「做一個有新增刪除打勾存檔還要漂亮的待辦清單」，它很可能做出九成，剩下一成有問題，你卻不知道是哪個環節出錯。
拆成小步驟，每一步都先確認能動，再往下一步。
而且每完成一步，就用 Git 存一個存檔點。這樣就算第四步 AI 把東西改壞了，你也可以退回第三步重來，不用從頭開始。
-->

---

# 如果不知道怎麼拆？請 AI 幫你拆

不確定步驟怎麼切也沒關係，可以先請 AI 幫忙規劃。

```text
我想做一個記帳網頁，可以記錄每天的收入和支出，
並顯示這個月的總結。

我是程式初學者，請先不要寫程式。
請幫我把這個目標拆成 5～7 個小步驟，
每一步都要是「做完就能在瀏覽器看到成果」的大小，
並說明每一步完成後我要怎麼確認它是對的。
```

<!--
如果你還不知道一個功能該怎麼拆，就直接請 AI 幫你拆。
這段 prompt 有兩個重點：第一，明確說「先不要寫程式」，避免 AI 一頭熱就開始動手；第二，要求每一步都是「做完就看得到成果」的大小，這樣你每一步都能驗收。
拿到步驟清單之後，你可以先調整順序、刪掉不需要的，再一步一步請它執行。
這其實就是下一段要講的 Plan 模式的精神：先規劃、再動手。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 先規劃再動手
# Plan Before You Build

<!--
接下來看 opencode 內建的規劃功能。
-->

---

# Plan 模式：先看藍圖再蓋房子

opencode 內建兩個主要 agent，用 **Shift+Tab** 切換（或輸入 `/agents` 選擇）。

| Agent | 會做的事 | 適合的時機 |
| --- | --- | --- |
| `plan` | 只分析、提出計畫，**不改檔案** | 新功能、大改動、不確定怎麼做的時候 |
| `build` | 實際修改檔案、執行指令 | 計畫確認後，開始動手 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>新手建議：</b> 只要任務會改到兩個以上的檔案，就先用 <code>plan</code> 討論。確認計畫沒問題再切到 <code>build</code>。
</div>

<!--
opencode 有兩個主要的工作模式，按 Shift+Tab 可以在它們之間切換。
plan 模式下，AI 只會分析跟提出計畫，不會動你的任何檔案，就像建築師先畫藍圖給你看。
build 模式下，AI 才會真的動手改檔案、跑指令。
新手最容易發生的狀況是：一開口 AI 就改了一堆檔案，改完才發現方向錯了。
所以我的建議是，只要任務會碰到兩個以上的檔案，就先切到 plan 討論，確認沒問題再切回 build 執行。
-->

---

# 規劃工作流程

```text
① [plan] 我想在待辦清單加上「依日期排序」的功能，
         請先告訴我你打算改哪些檔案、怎麼改，不要動手。

② AI 回覆計畫：要改 app.js 的 render 函式、新增 sortByDate…

③ [plan] 計畫 OK，但不要改 HTML 結構，排序按鈕放在清單上方就好。

④ 按 Shift+Tab 切到 build

⑤ [build] 照剛剛確認的計畫執行，做完告訴我怎麼測試。
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>看不懂計畫就問：</b> 計畫裡出現不懂的名詞，直接問「什麼是 render 函式？」。看不懂就同意，等於沒有規劃。
</div>

<!--
實際的流程大概是這樣。
第一步，在 plan 模式說明需求，並明確要它先不要動手。
第二步，AI 會告訴你打算改哪些檔案、怎麼改。
第三步是關鍵：你可以修正它的計畫，例如「不要動 HTML 結構」。
確認之後，按 Shift+Tab 切到 build，請它照著計畫執行。
最後提醒一點：計畫裡如果有看不懂的東西，一定要問清楚。看不懂就按同意，跟沒有規劃是一樣的。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 4. 讀懂 AI 的修改
# Review the Diff

<!--
最後一段，也是保護自己最重要的一段：檢查 AI 到底改了什麼。
-->

---

# 看懂 diff

AI 改完檔案後，會用 diff（差異）顯示改了什麼。`-` 是刪掉的，`+` 是新增的。

```diff
 function addTodo(text) {
-  todos.push(text);
+  if (text.trim() === "") return;
+  todos.push({ text: text, done: false, date: new Date() });
   save();
   render();
 }
```

| 符號 | 意思 |
| --- | --- |
| `-` 紅色 | 這一行被刪掉了 |
| `+` 綠色 | 這一行是新加的 |
| 沒有符號 | 沒有變動，只是讓你看前後文 |

<!--
AI 改完程式後，會用一種叫 diff 的格式告訴你改了什麼。
看 diff 很簡單：減號開頭、通常是紅色的，代表刪掉的；加號開頭、通常是綠色的，代表新加的；沒有符號的是前後文，沒有變動。
以這個例子來說，AI 把原本只存文字的 todos.push，改成存一個物件，裡面有文字、完成狀態跟日期，還加了一行檢查，空白內容就不新增。
你不需要每一行都完全看懂，但至少要看得出來：它改了哪裡、大概改了什麼。
-->

---

# 接受修改前的檢查清單

不需要看懂每一行程式，但這幾件事一定要確認。

| 檢查項目 | 為什麼重要 |
| --- | --- |
| 改到的檔案跟計畫一樣嗎？ | 改了不相關的檔案，常常是出錯的開始 |
| 有沒有大段程式被刪掉？ | AI 有時候會「順手」刪掉它覺得沒用的程式 |
| 有沒有安裝新套件？ | 新套件可能讓專案變複雜，或帶來安全風險 |
| 有沒有執行危險指令？ | 例如 `rm -rf`、`sudo`，要先問清楚再同意 |
| 實際測試過了嗎？ | AI 說「完成了」不代表真的能動，要自己打開來試 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>看不懂就請 AI 解釋：</b> 「請用初學者聽得懂的方式，逐段說明你剛剛改了什麼、為什麼要這樣改。」
</div>

<!--
這張清單建議大家每次都過一遍。
第一，改到的檔案跟計畫一樣嗎？如果計畫說只改 app.js，結果它連 index.html 都改了，就要問為什麼。
第二，有沒有大段程式被刪掉？AI 有時候會覺得某段程式沒用就順手刪掉，但那段可能是你需要的。
第三，有沒有裝新套件？第四，有沒有執行危險指令？這兩個都要先問清楚再同意。
最後也最重要：AI 說完成了，不代表真的能動。一定要自己打開瀏覽器實際測試一次。
-->

---
layout: default
---

# 練習：用四要素做一個小工具

### 任務說明

用今天學到的方法，請 opencode 做一個「倒數計時器」網頁。要求：

1. 先寫出包含**目標、背景、限制、驗收標準**的完整需求
2. 用 `plan` 模式請 AI 拆步驟、提出計畫，確認後再切到 `build` 執行
3. 每一步完成後檢查 diff，並在瀏覽器實際測試

<!--
這個練習要大家把今天四個技巧都用一次。
倒數計時器不大不小，剛好適合練習拆步驟。
記得一開始先在 plan 模式討論，不要一上來就叫它寫程式。
-->

---
layout: default
---

# 練習：用四要素做一個小工具

### 解題提示

1. 需求範例：「在 `timer.html` 做倒數計時器，只用 HTML/CSS/JS，可以輸入分鐘數、開始、暫停、重設，時間到要跳出提醒，不要安裝套件」
2. 在 `plan` 模式加一句：「我是初學者，請拆成 4～5 步，每步都要能在瀏覽器看到成果」
3. 每做完一步，對照檢查清單看 diff，打開瀏覽器測試，沒問題再請它做下一步
4. 最後請 AI「用初學者的方式解釋整份程式的運作流程」

<!--
提示第一點的需求範例，大家可以對照看看，四個要素是不是都有了：目標是倒數計時器，背景是 timer.html 跟純 HTML/CSS/JS，限制是不裝套件，驗收標準是開始、暫停、重設、時間到提醒都要能用。
最後一步請 AI 解釋整份程式，是為了確保你不只是做出來，也看得懂。這個習慣下一章會再深入談。
-->

---
layout: end
---

# 課程結束
### 下一步：程式出錯怎麼辦？學會跟 AI 一起除錯，並真正學懂程式！

<!--
今天我們學了怎麼跟 AI 說話：用四要素把需求講清楚、把大目標拆小、先用 plan 規劃再用 build 動手，最後檢查 diff、實際測試。
這些技巧會在之後每一次 vibe coding 都用到。
下一堂課，我們要來面對每個寫程式的人都會遇到的事：程式出錯了怎麼辦，以及怎麼用 AI 真正學懂程式。
-->
