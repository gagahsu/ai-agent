---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: opencode Skill 介紹、安裝與使用
routeAlias: ch04
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
    Agent Skills 介紹、安裝與使用
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「教 AI 一套固定 SOP，下次不用再從頭教一次」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
前面幾堂課，我們已經能請 opencode 幫忙寫程式了，但每次遇到比較固定、重複性高的流程，像是「怎麼發版本」「怎麼寫測試」，都要重新跟它解釋一次規則，很沒效率。
今天要介紹的 Agent Skills，就是幫 opencode「記住」這些固定流程的方法。
學完這堂課，我們會知道 Skill 是什麼、怎麼安裝、怎麼寫一份自己的 Skill。
-->

---
layout: default
---

# Outline

- **1. 什麼是 Agent Skills** — 可重複使用的技能說明書
- **2. Skill 的檔案結構** — SKILL.md 與資料夾規則
- **3. 安裝與存放位置** — 專案層級 vs 全域層級
- **4. 使用與權限控制** — 呼叫方式與 allow/deny/ask

<!--
今天四段：先了解 Skill 概念，再看它的檔案格式，接著學會安裝擺放位置，最後練習怎麼呼叫跟管理權限。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 什麼是 Agent Skills？
# Reusable Instructions

<!--
先來認識這個概念。
-->

---

# 什麼是 Agent Skills？

「可重複使用的操作說明書，opencode 會在需要時自動載入」，讓 AI 遵循固定的專業流程，不用每次重講一遍。

| 沒有 Skill | 有 Skill |
| --- | --- |
| 每次都要重新描述規則、步驟 | 寫一次，之後自動套用 |
| AI 容易漏掉團隊慣例 | 團隊 SOP 固化成文件 |
| 知識存在人的腦袋裡 | 知識存在專案裡，新成員也能用 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>比喻：</b> Skill 就像廚房裡貼在牆上的「標準食譜卡」，廚師（AI）需要做某道菜時，隨手抽出對應的食譜卡照著做，不用每次靠記憶現場摸索。
</div>

<!--
想像一個情境：每次要發新版本，你都要跟 AI 講一次「先看有哪些 PR 合併了、幫我整理成 changelog、版本號要照 semver 規則往上加」，講久了很煩，而且不同人講的規則可能還會有落差。
Skill 要解決的就是這件事：把這套固定流程寫成一份文件，存進專案或電腦裡，opencode 會自動偵測到有哪些 Skill 可以用，需要的時候自己載入內容來參考，不用我們每次重講。
就像餐廳廚房牆上貼的標準食譜卡，寫死了份量、步驟，換一個廚師來做，味道也不會跑掉。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. Skill 的檔案結構
# SKILL.md

<!--
來看看一份 Skill 實際長什麼樣子。
-->

---

# SKILL.md 檔案規則

每個 Skill 是一個資料夾，裡面放一份 `SKILL.md`，開頭用 YAML frontmatter 寫基本資訊。

| 欄位 | 重要性 | 說明 |
| --- | --- | --- |
| `description` | ⭐ 一定要寫 | 說明用途，AI 靠這段判斷何時該用；沒寫的話 AI 看不到這個 Skill |
| `name` | 建議寫 | 顯示用的名稱 |
| `disable-model-invocation` | 選填 | 設成 `true`，AI 就不會自動使用，只能由你手動叫用 |
| `metadata` | 選填 | 自訂的資訊 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>Skill 的 ID 是資料夾名稱：</b> 例如放在 <code>.opencode/skills/caveman/SKILL.md</code>，這個 Skill 的 ID 就是 <code>caveman</code>。權限設定、手動叫用都是用這個 ID。
</div>

<!--
一份 Skill 其實就是一個資料夾加一份 Markdown 文件。
最重要的欄位是 description，它決定了 AI「什麼時候該想到用這個 Skill」，寫得越具體，AI 判斷得越準。如果 description 沒寫，這個 Skill 甚至不會被 AI 看見。
name 只是顯示用的名稱。Skill 真正的 ID 是它的資料夾名稱，例如資料夾叫 caveman，ID 就是 caveman，之後設定權限、手動叫用都是用這個 ID。
如果有些 Skill 你不希望 AI 自己決定要不要用，可以加上 disable-model-invocation: true，這樣只有你手動叫用時才會載入。
-->

---

# SKILL.md — 範例

以本堂課全程示範用的「caveman」壓縮語氣 Skill 為例，存成 `.opencode/skills/caveman/SKILL.md`：

```yaml
---
name: caveman
description: 超壓縮溝通模式，砍掉贅字但保留完整技術內容，
  可省下約 75% token。支援 lite/full/ultra 三種強度。
  使用者說「講話省一點」「caveman 模式」時使用。
metadata:
  intensity: lite | full | ultra
---
## 我會做的事
- 去掉冠詞、贅字、客套話，句子縮成片語
- 依 intensity 分級調整壓縮程度
- 遇到安全警告、需使用者確認的動作，自動切回正常語氣

## 使用時機
使用者要求省 token、講話簡短，或輸入 @caveman 時使用。
```

<!--
這份範例就是我們接下來整堂課都會拿來示範的 caveman Skill，description 寫得很白話：AI 一看到「使用者想省 token、想講話簡短」，就知道要載入這份說明。
下面的內文則是實際的操作邏輯，寫法就跟你在教一位新同事怎麼做這件事一樣，越具體越好，包含要做哪些事、什麼時候該用它、什麼時候該例外。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 安裝與存放位置
# Where Skills Live

<!--
寫好了要放在哪裡，opencode 才找得到？
-->

---

# Skill 的搜尋路徑

opencode 會依序尋找以下路徑，分成「專案層級」與「全域層級」。

| 層級 | 路徑 |
| --- | --- |
| 專案層級 | `.opencode/skills/<id>/SKILL.md` |
| 全域層級 | `~/.config/opencode/skills/<id>/SKILL.md` |
| 相容專案層級 | `.claude/skills/<id>/SKILL.md`、`.agents/skills/<id>/SKILL.md` |
| 相容全域層級 | `~/.claude/skills/<id>/SKILL.md`、`~/.agents/skills/<id>/SKILL.md` |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>安裝就是「放對資料夾」：</b> Skill 沒有專屬安裝指令，只要把資料夾放進上述任一路徑，opencode 下次啟動就會自動偵測到。
</div>

<!--
Skill 的「安裝」其實非常單純：不需要下載或執行任何安裝指令，只要把整個資料夾放進 opencode 會搜尋的路徑，它就自動生效了。
專案層級放在 .opencode/skills 底下，適合只給這個專案用、要進版本控制跟團隊共用的 Skill。
全域層級放在使用者家目錄的 .config/opencode/skills 底下，這樣不管開哪個專案都能用，適合個人習慣的通用技能。
opencode 也相容 .claude/skills 和 .agents/skills 這兩種資料夾，如果你原本就有給 Claude Code 或其他工具用的 Skill，放在原本的位置，opencode 也讀得到。
-->

---

# 補充：從其他地方載入 Skill

除了放進固定資料夾，也可以在 `opencode.jsonc` 用 `skills` 陣列指定額外的來源。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "skills": [
    "../team-shared/skills",            // 本機的其他資料夾
    "https://example.com/skills/"       // 網路上的 Skill 目錄（需提供 index.json）
  ]
}
```

<!--
除了放進固定的資料夾，還可以在設定檔裡用 skills 陣列指定額外的來源。
可以是電腦上的其他資料夾，例如團隊共用的 Skill 放在另一個 repo；也可以是網路上的 Skill 目錄，只要對方有提供 index.json 清單。
這樣就不用每個專案都複製一份，等於大家共用同一個食譜櫃。這個用法大家先知道就好，範例網址要換成實際的位置。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 4. 使用與權限控制
# Invoke & Permission

<!--
放好之後，實際怎麼呼叫、怎麼管理？
-->

---

# 呼叫方式 — 以 caveman 為例

opencode 會透過內建的 `skill` 工具自動判斷並載入合適的 Skill；也可以在對話中打 `@caveman` 手動指名。

```text
使用者：講話幫我省一點，開 caveman full 模式

AI（載入 caveman skill 後）：
Bug 在 auth middleware。Token 過期檢查用了 `<` 該用 `<=`。修正：
```

<!--
這一頁我們實際看一次 caveman skill 被觸發的樣子。
使用者只要說出「講話省一點」「caveman 模式」這類字眼，opencode 就會自己比對所有已安裝 Skill 的 description，發現 caveman 這份符合，自動載入內文的壓縮規則，之後的回覆就會照這套規則變成短句、去贅字，但技術內容完全不打折。
整個過程使用者完全不用打開 SKILL.md 看內容，這就是 Skill「隨插即用」的價值。
如果想確定用到某個 Skill，也可以直接在對話裡打 @ 加上 Skill 的 ID，例如 @caveman，手動指名載入。
-->

---

# 權限設定

有些 Skill 比較敏感、或還在測試階段，可以用權限規則限制何時能自動套用。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  // 規則由上往下比對，後面的規則優先
  "permissions": [
    { "action": "skill", "resource": "*", "effect": "allow" },
    { "action": "skill", "resource": "caveman*", "effect": "ask" },
    { "action": "skill", "resource": "internal-*", "effect": "deny" }
  ]
}
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>舊版寫法已淘汰：</b> 網路上常見的 <code>"permission": { "skill": {...} }</code> 是舊版（V1）格式。新版用 <code>permissions</code> 陣列，也可以寫在個別 agent 的設定裡（<code>agents.&lt;id&gt;.permissions</code>）。
</div>

<!--
如果團隊裡有些 Skill 比較敏感，不想讓它隨便自動執行，可以用 permissions 規則管理。每一條規則有三個欄位：action 是動作類型，這裡是 skill；resource 是 Skill 的 ID，可以用星號當萬用字元；effect 是效果，allow 直接放行，deny 完全禁止，ask 每次使用前先問你。
規則是由上往下比對，越後面的規則越優先。像範例這樣，先全部 allow，再把 caveman 開頭的設成 ask，等於「AI 想切換成省話語氣前，要先問過我同不同意」，而內部測試用的 internal- 開頭則直接 deny。
提醒大家，網路上很多教學還是舊版的寫法，看到 permission 單數、後面接物件的，那是舊格式，新版要用 permissions 陣列。
-->

---

# 確認 Skill 有被抓到

放好資料夾之後，可以用這兩個方法確認 opencode 有沒有偵測到。

```text
# 方法一：直接問 AI
你目前有哪些可以用的 skill？

# 方法二：用 @ 手動指名，看它有沒有照 Skill 的規則回答
@caveman 解釋一下這個專案的資料夾結構
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>新增或修改 Skill 後：</b> 建議重新啟動 opencode 或開新對話（<code>/new</code>），確保讀到最新的內容。
</div>

<!--
裝完 Skill 心裡毛毛的、不確定有沒有生效，很正常，這裡教大家兩個檢查方式。
第一個最簡單，直接問 AI 目前有哪些可以用的 skill，它看得到每個 Skill 的 description，會列給你看。
第二個是用 @ 手動指名，例如打 @caveman 再接一個問題，如果回答變成精簡的省話語氣，就代表 Skill 有被讀到。
另外，新增或修改 Skill 之後，建議重新啟動 opencode 或開個新對話，確保它讀到的是最新的內容。
-->

---
layout: default
---

# 練習：體驗並仿寫 caveman Skill
### 任務說明

先把本堂課的 caveman 範例存成 `.opencode/skills/caveman/SKILL.md`，在 opencode 對話框打字觸發它，感受壓縮語氣的效果；接著仿照它的結構，寫一份 `.opencode/skills/pr-checklist/SKILL.md`，內容是「開 PR 前的自我檢查清單」（跑哪些測試指令、commit message 格式規則）。

---
layout: default
---

# 練習：體驗並仿寫 caveman Skill
### 解題提示

1. 先打「講話幫我省一點」或「caveman 模式」，觀察 AI 回覆語氣的變化，確認 Skill 真的被載入了
2. 仿照 caveman 的 frontmatter 寫法，想清楚 `description` 要怎麼寫，才能讓 AI 準確判斷「開 PR 前」該用你的 Skill
3. 內文條列出檢查項目，例如跑 lint、跑測試、確認 commit message 格式
4. 存到 `.opencode/skills/pr-checklist/SKILL.md`，資料夾名稱 `pr-checklist` 就是這個 Skill 的 ID
5. 啟動 opencode，跟它說「我要開 PR 了，幫我檢查一下」，看它有沒有自動套用這份 Skill

<!--
這個練習分兩步：先把範例的 caveman skill 存起來，體驗它長什麼感覺，再仿照同樣的結構自己寫一份。
重點提示一下，description 是關鍵中的關鍵，寫得夠具體，AI 才會在對的時機把這份 Skill 抽出來用，就跟 caveman 那份寫「使用者說想省 token 時觸發」一樣明確。
-->

---
layout: end
---

# 課程結束
### 下一步：認識 MCP，讓 opencode 接上外部工具與服務！

<!--
今天我們認識了 Agent Skills：它是什麼、怎麼寫、放在哪裡、怎麼控制使用權限。
大家可以練習寫一份自己團隊常用的 SOP 當作 Skill，讓 opencode 幫忙記住。
下一堂課，我們要介紹 MCP，讓 opencode 除了寫程式，還能連上 GitHub、資料庫等外部工具！
-->
