---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: 用 AGENTS.md 打造 AI 家教
routeAlias: ch08
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
    用 AGENTS.md 打造 AI 家教
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「說一次就好，AI 每次都會記得」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
前兩堂課，我們學了很多跟 AI 說話的技巧，例如「假設我是初學者」「先解釋原因再修改」。
但如果每次開新對話都要重講一遍，實在很累。
這堂課要教大家寫一份 AGENTS.md，把這些偏好寫下來，AI 每次對話都會自動讀，變成一位專屬於你的 AI 家教。
-->

---
layout: default
---

# Outline

- **1. 什麼是 AGENTS.md** — AI 每次都會讀的說明書
- **2. 放在哪裡** — 全域與專案兩個層級
- **3. 寫一份 AI 家教版 AGENTS.md** — 範本與寫作原則
- **4. 進階：自訂指令** — 把常用的 prompt 變成斜線指令

<!--
今天分四段。
先認識 AGENTS.md 是什麼。
再看它可以放在哪裡，全域跟專案有什麼差別。
第三段是重點，我們一起寫一份 AI 家教版的 AGENTS.md。
最後介紹一個進階技巧：把常用的 prompt 變成自己的斜線指令。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 什麼是 AGENTS.md？
# Rules for Every Session

<!--
先來認識 AGENTS.md。
-->

---

# 什麼是 AGENTS.md？

「一份用 Markdown 寫的說明書，opencode 每次開始對話都會自動讀它」，裡面寫的規則會一直有效。

| 沒有 AGENTS.md | 有 AGENTS.md |
| --- | --- |
| 每次開新對話都要重講偏好 | 寫一次，每次對話自動套用 |
| AI 用它自己習慣的方式回答 | AI 照你的程度和喜好回答 |
| 專案規則只存在你的腦袋裡 | 規則寫在專案裡，換電腦、換人都能用 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>跟 Skill 的差別：</b> AGENTS.md 是「每次都會讀」的基本規則；Skill 是「需要時才載入」的專門流程。每次都要遵守的寫在 AGENTS.md，特定任務才用到的寫成 Skill。
</div>

<!--
AGENTS.md 就是一份用 Markdown 寫的說明書，opencode 每次開始對話都會自動讀它，所以裡面寫的規則會一直有效。
這跟我們之前學的 Skill 有點像，但差別在於：AGENTS.md 是每次都會讀的基本規則，Skill 是需要的時候才載入的專門流程。
舉例來說，「請用繁體中文回答」每次都要遵守，就寫在 AGENTS.md；「開 PR 前的檢查清單」只有開 PR 時才需要，就寫成 Skill。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 放在哪裡
# Global vs Project

<!--
AGENTS.md 可以放在兩個地方，效果不太一樣。
-->

---

# 全域與專案兩個層級

| 層級 | 位置 | 適合寫什麼 |
| --- | --- | --- |
| 全域 | `~/.config/opencode/AGENTS.md` | 個人偏好：語言、你的程度、教學方式 |
| 專案 | 專案根目錄的 `AGENTS.md` | 專案規則：用什麼技術、檔案結構、不能動的東西 |

- 兩份都會讀：先讀全域，再讀專案的
- 專案的 `AGENTS.md` 建議用 Git 一起存起來，換電腦也能用
- 如果資料夾裡沒有 `AGENTS.md`，opencode 會改讀 `CLAUDE.md`（相容 Claude Code）

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>不要放機密：</b> 專案的 AGENTS.md 通常會跟程式碼一起上傳，API key、密碼絕對不能寫在裡面。
</div>

<!--
AGENTS.md 有兩個層級。
全域的放在 ~/.config/opencode/AGENTS.md，不管開哪個專案都會讀，適合寫個人偏好，像是用什麼語言回答、你目前的程度、喜歡怎麼被教。
專案的放在專案根目錄，只有在這個專案才會讀，適合寫這個專案的規則，像是用什麼技術、哪些檔案不能動。
兩份都會讀，先讀全域、再讀專案。
另外，如果你之前用過 Claude Code，有寫 CLAUDE.md，opencode 在找不到 AGENTS.md 的時候也會讀它。
最後提醒：專案的 AGENTS.md 通常會上傳到 GitHub，所以千萬不要把 API key 寫在裡面。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 寫一份 AI 家教版 AGENTS.md
# Your Personal Tutor

<!--
接下來是今天的重點，一起寫一份 AI 家教版的 AGENTS.md。
-->

---

# 全域 AGENTS.md 範本（個人偏好）

```markdown {all|3-5|7-11|13-16}
# 關於我

- 我是程式初學者，正在用 vibe coding 學寫網頁
- 我懂基本的 HTML，JavaScript 還不太熟
- 請一律用繁體中文回答

# 教學方式

- 寫完程式後，用初學者聽得懂的方式說明你做了什麼、為什麼這樣做
- 出現新的專有名詞時，用一句話解釋它
- 一次只做一個小步驟，做完告訴我怎麼測試

# 安全規則

- 安裝任何套件之前，先告訴我為什麼需要，等我同意
- 不要刪除檔案，除非我明確要求
```

<!--
這是一份全域 AGENTS.md 的範本，分成三段。
第一段「關於我」，告訴 AI 你的程度。這一段非常重要，AI 知道你是初學者，就會調整解釋的方式。
第二段「教學方式」，把我們前兩堂課學到的家教技巧寫進去，像是寫完要解釋、遇到新名詞要說明、一次只做一小步。
第三段「安全規則」，要求它裝套件、刪檔案之前都要先問你。
大家可以直接拿這份當起點，再依照自己的狀況調整。
-->

---

# 專案 AGENTS.md 範本（專案規則）

```markdown
# 專案說明

這是一個待辦清單網頁，給自己每天記錄要做的事。

# 技術

- 只用 HTML、CSS、原生 JavaScript，不使用框架
- 資料存在瀏覽器的 localStorage

# 檔案結構

- `index.html`：畫面結構
- `style.css`：樣式
- `app.js`：所有功能的程式

# 規則

- 不要新增其他 JavaScript 檔案，功能都寫在 `app.js`
- 每個函式上方用一行中文註解說明用途
```

<!--
這是專案層級的範本，寫的是這個專案自己的規則。
專案說明讓 AI 知道這個專案要做什麼。
技術那一段很重要，明確說「不使用框架」，AI 就不會突然幫你裝 React。
檔案結構告訴 AI 每個檔案負責什麼，它改程式時就知道要去哪裡改。
最後的規則可以依照你的需求寫，像這裡要求每個函式都要有中文註解，對初學者看懂程式很有幫助。
-->

---

# 寫 AGENTS.md 的原則

| 原則 | ❌ 不好的寫法 | ✅ 好的寫法 |
| --- | --- | --- |
| 具體 | 「程式要寫好一點」 | 「每個函式上方加一行中文註解」 |
| 簡短 | 寫了三頁的規則 | 只寫最重要的 10～20 條 |
| 可以檢查 | 「要注意安全」 | 「安裝套件前先問我」 |
| 持續更新 | 寫完就不管 | AI 犯了同樣的錯兩次，就加一條規則 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>請 AI 幫你寫：</b> 「請根據這個專案的檔案，幫我寫一份 AGENTS.md，說明專案用途、技術和檔案結構。寫完先給我看，不要直接存檔。」
</div>

<!--
寫 AGENTS.md 有四個原則。
第一，要具體。「程式要寫好一點」AI 不知道怎麼做，「每個函式加一行中文註解」它就做得到。
第二，要簡短。每次對話都會讀這份文件，寫太長會佔用上下文，也會讓重點被淹沒。
第三，要可以檢查。你要能看得出 AI 有沒有遵守。
第四，要持續更新。我自己的習慣是：AI 同樣的錯犯了兩次，我就加一條規則進去。
如果不知道從哪裡開始，也可以請 AI 根據專案內容幫你寫一份初稿。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 4. 進階：自訂指令
# Custom Commands

<!--
最後介紹一個進階技巧：自訂斜線指令。
-->

---

# 把常用 prompt 變成斜線指令

前幾堂課常用的 prompt，可以存成自己的指令，打 `/指令名稱` 就能用。

```markdown
---
description: 用初學者的方式解釋一個檔案
agent: plan
---

請逐段解釋 $ARGUMENTS，假設我是程式初學者：
1. 每一段用一兩句話說明它在做什麼
2. 出現新名詞時用生活中的例子解釋
3. 最後出 3 題選擇題考我
```

| 存放位置 | 效果 |
| --- | --- |
| `.opencode/commands/explain.md` | 只在這個專案能用 `/explain` |
| `~/.config/opencode/commands/explain.md` | 所有專案都能用 |

<!--
上一堂課我們學了很多家教型的 prompt，每次都要打一長串很麻煩。
opencode 可以把這些 prompt 存成自訂指令。
做法是在 commands 資料夾裡新增一個 Markdown 檔，檔名就是指令名稱。這裡存成 explain.md，就可以打 /explain 來用。
$ARGUMENTS 會被換成你打在指令後面的內容，例如 /explain @app.js，就會解釋 app.js。
frontmatter 裡的 agent: plan，讓這個指令在 plan 模式下執行，AI 只解釋、不會改檔案。
-->

---
layout: default
---

# 練習：打造你的 AI 家教

### 任務說明

1. 參考範本，寫一份屬於你自己的全域 `~/.config/opencode/AGENTS.md`
2. 在自己的練習專案裡，請 AI 幫你寫一份專案 `AGENTS.md` 初稿，自己檢查修改後再存檔
3. 建立一個 `/explain` 自訂指令
4. 開一個新對話，用 `/explain @某個檔案` 測試，確認 AI 有照你的規則回答

<!--
這個練習要大家打造自己的 AI 家教。
全域的 AGENTS.md 請依照自己真實的程度跟喜好來寫，不用照抄範本。
專案的 AGENTS.md 可以請 AI 幫忙寫初稿，但記得自己看過、修改後再存。
最後開一個新對話測試，這一步很重要，因為只有新對話才能確認 AI 真的有讀到你的規則。
-->

---
layout: default
---

# 練習：打造你的 AI 家教

### 解題提示

1. 「關於我」那段要寫出真實程度，例如「HTML 會一點，JavaScript 完全沒學過」
2. 請 AI 寫初稿時加一句「先給我看，不要直接存檔」
3. 自訂指令的檔名就是指令名稱，`explain.md` 對應 `/explain`
4. 測試時觀察：AI 有沒有用繁體中文？有沒有解釋新名詞？有沒有出題考你？沒做到的就把規則寫得更具體

<!--
最後一點提示最重要：如果 AI 沒有照規則做，通常是規則寫得不夠具體。
把它改得更具體，再開新對話測試，反覆幾次，你就會有一份真正好用的 AI 家教設定。
-->

---
layout: end
---

# 課程結束
### 下一步：認識各種 AI 寫程式工具，知道什麼時候用哪一個！

<!--
今天我們學了 AGENTS.md：它是 AI 每次都會讀的說明書，可以放在全域或專案，也一起寫了一份 AI 家教版的範本，最後學會用自訂指令把常用 prompt 變成斜線指令。
下一堂課，我們會跳出 opencode，認識市面上其他 AI 寫程式工具，了解它們的差別跟適合的使用時機。
-->
