---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: 除錯與學習
routeAlias: ch07
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
    除錯與學習
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「不只要做得出來，還要看得懂」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
寫程式一定會出錯，就算是 AI 寫的也一樣。
這堂課有兩個目標：第一，程式出錯時，知道怎麼跟 AI 合作把它修好；第二，學會用 AI 當家教，讓自己真的看懂程式，而不是只會一直請 AI 幫忙。
-->

---
layout: default
---

# Outline

- **1. 看懂錯誤訊息** — 錯誤訊息的三個部分
- **2. 把錯誤回報給 AI** — 一份好的錯誤回報
- **3. AI 鬼打牆怎麼辦** — 認出徵兆、換個方法
- **4. 用 AI 學懂程式** — 把 AI 變成你的家教

<!--
今天分四段。
先學怎麼看錯誤訊息，不用看懂全部，抓到三個重點就好。
再學怎麼把錯誤完整回報給 AI。
第三段很實用：AI 修了好幾次都修不好的時候，該怎麼辦。
最後一段，學怎麼用 AI 真正學懂程式。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 看懂錯誤訊息
# Read the Error

<!--
先從錯誤訊息開始。很多初學者看到一整片紅字就慌了，其實它是在幫你。
-->

---

# 錯誤訊息的三個部分

錯誤訊息不是在罵你，而是在告訴你「哪裡、出了什麼事」。

```text
Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')
    at app.js:12:7
```

| 部分 | 範例 | 意思 |
| --- | --- | --- |
| 錯誤類型 | `TypeError` | 用錯了資料的型別 |
| 錯誤說明 | `Cannot read properties of null` | 想對一個「不存在」的東西做事 |
| 發生位置 | `app.js:12:7` | `app.js` 第 12 行、第 7 個字 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>網頁的錯誤在哪看：</b> 在瀏覽器按 <code>F12</code>（Mac 是 <code>Cmd+Option+I</code>）打開開發者工具，切到 <code>Console</code> 分頁，紅字就是錯誤訊息。
</div>

<!--
錯誤訊息通常可以拆成三個部分。
第一個是錯誤類型，這裡是 TypeError，大概是用錯了資料的型別。
第二個是說明，Cannot read properties of null，意思是程式想對一個不存在的東西做事。以這個例子來說，通常是程式要找某個按鈕，但網頁上根本沒有這個按鈕，或是名字打錯了。
第三個是位置，app.js 第 12 行，這是最有用的資訊，直接告訴你去哪裡找問題。
網頁的錯誤訊息要在瀏覽器的開發者工具裡看，按 F12 打開，切到 Console 分頁。
-->

---

# 錯誤訊息在哪裡？

不同類型的程式，錯誤訊息出現的地方不一樣。

| 你在做的東西 | 錯誤訊息在哪裡 |
| --- | --- |
| 網頁（HTML / JavaScript） | 瀏覽器開發者工具的 `Console` 分頁 |
| 在終端機執行的程式 | 終端機畫面上 |
| 用 `npm run dev` 啟動的專案 | 終端機**和**瀏覽器 Console 都要看 |
| 畫面沒錯誤但結果不對 | 沒有錯誤訊息，要自己描述「預期 vs 實際」 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>在 opencode 裡直接執行：</b> 在對話框開頭打 <code>!</code> 可以進入 shell 模式執行指令（例如 <code>!npm run dev</code>），輸出會留在對話裡，AI 也看得到。按 Esc 離開。
</div>

<!--
錯誤訊息出現的地方，取決於你在做什麼。
網頁的錯誤在瀏覽器 Console，終端機程式的錯誤在終端機。
如果是用 npm run dev 啟動的專案，終端機跟瀏覽器兩邊都可能有錯誤，兩邊都要看。
最麻煩的是第四種：沒有任何錯誤，但結果不對。這時候就要靠你自己描述「我預期看到什麼、實際看到什麼」。
另外分享一個 opencode 的技巧：在對話框開頭打驚嘆號，可以直接執行終端機指令，執行結果會留在對話裡，AI 也看得到，就不用自己複製貼上了。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 把錯誤回報給 AI
# Report the Bug

<!--
看到錯誤之後，下一步是把它完整交給 AI。
-->

---

# 好的錯誤回報

只說「壞掉了」，AI 只能亂猜。照這個格式回報，AI 一次修好的機率高很多。

```text
【我做了什麼】打開 index.html，按下「新增」按鈕
【我預期】清單多出一個項目
【實際發生】沒有反應，Console 出現以下錯誤：

Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')
    at app.js:12:7

【最近的修改】剛剛請你把按鈕的 id 從 add-btn 改成 addButton

請先說明你認為的原因，再修改。
```

<!--
這是一份好的錯誤回報範本，有四個部分。
我做了什麼：讓 AI 知道怎麼重現問題。
我預期跟實際發生：讓 AI 知道差距在哪裡。
完整錯誤訊息：記得是完整複製，不要自己摘要。
最近的修改：這個很常被忽略，但很多錯誤都是上一步改出來的。像這個例子，按鈕 id 改了，但 app.js 裡找按鈕的程式還是用舊名字，所以才找不到。
最後一句「先說明原因再修改」，讓 AI 先想清楚，也讓你學到為什麼會錯。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. AI 鬼打牆怎麼辦？
# When AI Gets Stuck

<!--
接下來談一個大家一定會遇到的狀況：AI 修了好幾次，問題還是在。
-->

---

# 認出鬼打牆的徵兆

AI 也會卡住。出現以下狀況時，繼續叫它「再試一次」通常沒用。

| 徵兆 | 例子 |
| --- | --- |
| 同一個錯誤修了 3 次以上 | 每次都說「已修正」，但錯誤一樣 |
| 修好 A，壞掉 B | 越修，壞掉的地方越多 |
| 改來改去又改回原樣 | 第五次的修改跟第二次一樣 |
| 開始做你沒要求的大改 | 突然要重寫整個檔案、換框架 |
| 回覆越來越慢、越來越短 | 對話太長，上下文快滿了 |

<!--
AI 卡住是很正常的事，重點是要早點發現。
最常見的徵兆是：同一個錯誤修了三次以上，每次都說修好了，結果還是一樣。
或是修好 A 壞掉 B，越修越多問題。
還有一種是 AI 開始做你沒要求的大改，像是要重寫整個檔案，這通常代表它已經失去方向了。
看到這些徵兆，不要再說「再試一次」，要換個方法。
-->

---

# 鬼打牆的對策

| 對策 | 怎麼做 |
| --- | --- |
| 🔙 退回存檔點 | 用 `/undo`，或用 Git 回到上一個正常的版本 |
| 🆕 開新對話 | 用 `/new` 開新 session，只帶入必要的資訊重新描述問題 |
| 🔍 先查原因，不要急著修 | 「先不要改程式，請在可疑的地方加 `console.log`，幫我確認問題在哪」 |
| ✂️ 把問題縮小 | 「先只讓按鈕按下去會跳出 alert」，確認最小的部分能動 |
| 🔄 換模型 | 用 `/models` 換一個模型，不同模型擅長的地方不一樣 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>為什麼開新對話有用：</b> 對話越長，AI 要記的東西越多，前面錯誤的嘗試也會干擾它。換一個乾淨的對話，常常一次就解決。
</div>

<!--
遇到鬼打牆，有五招可以用。
第一，退回存檔點。用 /undo 或 Git 回到上一個正常的版本，至少不會越修越糟。
第二，開新對話。對話太長時，前面那些失敗的嘗試會一直干擾 AI，開一個乾淨的對話重新描述，常常一次就好。
第三，先查原因、不要急著修。請 AI 在可疑的地方加 console.log 印出資訊，先確定問題在哪。
第四，把問題縮小，先讓最小的部分能動，再一步一步加回去。
第五，換一個模型試試看，不同模型擅長的東西不一樣。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 4. 用 AI 學懂程式
# AI as Your Tutor

<!--
最後一段，我覺得是整堂課最重要的部分：怎麼用 AI 真正學懂程式。
-->

---

# 「做得出來」不等於「學會了」

只會叫 AI 寫程式，遇到它修不好的問題，你就完全沒辦法了。

| 只讓 AI 寫 | 讓 AI 教你 |
| --- | --- |
| 程式能動，但不知道為什麼 | 知道每一段在做什麼 |
| AI 卡住你也卡住 | 能自己判斷問題大概在哪 |
| 不知道 AI 改得對不對 | 看得出 AI 的修改合不合理 |
| 每個專案都從零開始問 | 越做越快，問的問題越來越精準 |

<!--
很多人用 AI 寫程式，做出了東西卻完全看不懂。這在一開始沒什麼問題，但很快就會碰到天花板。
當 AI 卡住的時候，你也跟著卡住；AI 改錯了，你也看不出來。
反過來，如果你一邊做一邊學懂，就能自己判斷問題在哪、AI 的修改合不合理，而且會越做越快。
所以接下來教大家幾個把 AI 變成家教的方法。
-->

---

# 把 AI 變成家教的 prompt

| 目的 | 可以這樣問 |
| --- | --- |
| 📖 逐段解釋 | 「請逐段解釋 `@app.js`，每段用一兩句話說明它在做什麼，假設我是程式初學者」 |
| 🧩 用比喻 | 「用生活中的例子解釋什麼是函式、什麼是變數」 |
| ❓ 解釋為什麼 | 「為什麼這裡要用 `localStorage`？不用的話會怎樣？」 |
| 📝 出題考我 | 「根據這份程式出 3 題選擇題考我，我答完再告訴我對錯和原因」 |
| 🔧 我先試，你再看 | 「我想自己把按鈕改成藍色，先不要告訴我答案，給我提示就好」 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>學習時用 plan 模式：</b> 在 <code>plan</code> 模式下問問題，AI 只會解釋、不會動手改檔案，避免它「好心」直接幫你做完。
</div>

<!--
這裡整理了五種把 AI 變成家教的問法。
逐段解釋是最基本的，記得加上「假設我是初學者」，它就會用比較白話的方式說明。
用比喻很適合理解抽象的概念。
解釋為什麼可以幫助你理解設計上的選擇。
出題考我是檢查自己有沒有真的懂的好方法。
最後一個「我先試，你再看」最有效：先自己動手改，卡住了再請 AI 給提示，而不是直接給答案。
小提醒：學習的時候切到 plan 模式，AI 就不會好心直接幫你把東西改完。
-->

---

# 學習迴圈

每完成一個小功能，花 5 分鐘走一次這個迴圈。

| 步驟 | 做什麼 |
| --- | --- |
| 1. 做 | 請 AI 完成一個小功能 |
| 2. 問 | 請 AI 解釋剛剛的程式碼 |
| 3. 改 | 自己動手做一個小修改（換顏色、改文字、加一個條件） |
| 4. 考 | 請 AI 出題考你，或請它檢查你的修改 |
| 5. 記 | 把學到的一個新概念寫進自己的筆記 |

<!--
最後把這些方法串成一個學習迴圈。
每做完一個小功能，就花五分鐘走一次：請 AI 做、請它解釋、自己改一點東西、請它考你，最後寫一句筆記。
自己動手改的這一步特別重要，就算只是換個顏色、改一句文字，你也會開始知道「程式的哪裡控制什麼」。
持續這樣做，幾個專案之後，你會發現自己看得懂的東西越來越多。
-->

---
layout: default
---

# 練習：修一個 bug，學一個概念

### 任務說明

1. 請 opencode 在 `@index.html` 故意埋一個 bug（例如把按鈕的 id 打錯），**不要告訴你是哪裡**
2. 打開瀏覽器，用 F12 找到錯誤訊息，自己先猜問題在哪
3. 用「好的錯誤回報」格式把問題交給 AI，請它先解釋原因再修改
4. 修好後，請 AI 出 3 題考你這次學到的概念

<!--
這個練習很好玩：請 AI 故意幫你埋一個 bug，然後你自己當偵探把它找出來。
重點在第二步，先自己看錯誤訊息猜看看，猜錯也沒關係。
然後用今天學的格式回報給 AI，最後請它出題考你，確認你真的懂了。
-->

---
layout: default
---

# 練習：修一個 bug，學一個概念

### 解題提示

1. 埋 bug 的 prompt：「請在這個專案埋一個初學者常見的 bug，不要告訴我在哪裡，只告訴我症狀」
2. 錯誤訊息的「發生位置」會告訴你檔案和行數，先去那一行看看
3. 回報時記得附上完整錯誤訊息和「最近的修改」
4. 如果 AI 修了兩次還沒好，試試開新對話（`/new`）重新描述

<!--
提示第一點的 prompt 可以直接用，也可以加上「請埋一個跟 JavaScript 選取元素有關的 bug」來指定類型。
大家記得，找 bug 是程式設計師每天都在做的事，越早習慣，之後就越不怕。
-->

---
layout: end
---

# 課程結束
### 下一步：寫一份 AGENTS.md，打造專屬於你的 AI 家教！

<!--
今天我們學了看懂錯誤訊息的三個部分、怎麼寫一份好的錯誤回報、AI 鬼打牆時的五個對策，以及怎麼用 AI 當家教真正學懂程式。
下一堂課，我們要把這些偏好寫進 AGENTS.md，讓 AI 每次對話都自動照你的方式教你、幫你。
-->
