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

- **1. 什麼是 Agent Skills** — 不是 Prompt，是 Skill
- **2. Skill 的檔案結構** — SKILL.md 與資料夾規則
- **3. 安裝與存放位置** — 專案層級 vs 全域層級
- **4. 使用與權限控制** — 呼叫方式與 allow/deny/ask
- **5. Production-grade Skill** — 一支能上線的 Skill 知道哪五件事
- **6. Skill 的進化五層級** — 你現在在哪一層？
- **7. 從一支到一整套** — 測試、Benchmark、版本與治理

<!--
今天分成七段：前四段是入門，先了解 Skill 概念、檔案格式、擺放位置跟權限管理，做出一支「可以用」的 Skill。
後三段往上加一層：一支真正能上線的 Skill 該具備哪些要素、Skill 從陽春到工程化總共有五個層級、以及當你手上有幾十支 Skill 時，會遇到哪些管理問題。
先講重點：從「可以用」到「穩定、可測、可管理」，中間還差很多層，今天會帶大家看到方向。
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

# 不是 Prompt，是 Skill

兩者都是「用文字指揮 AI」，差別在於**能不能被重複使用、被管理**。

| 比較項目 | Prompt | Skill |
| --- | --- | --- |
| 存在哪裡 | 對話視窗裡，關掉就沒了 | 專案／電腦的資料夾裡 |
| 重複使用 | 每次重打或自己存記事本 | AI 自動判斷、自動載入 |
| 版本控管 | 沒辦法，改了就蓋掉 | 進 Git，可比對、可回溯 |
| 團隊共用 | 靠口耳相傳、複製貼上 | clone 專案就有，全員一致 |
| 可測試性 | 只能憑感覺「好像變好了」 | 可以寫測試驗證行為 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>一句話總結：</b> Prompt 是「這一次要你怎麼做」，Skill 是「把你的方法變成可以被 AI 重複使用的能力」。
</div>

<!--
很多人學到這裡會問：這跟我把常用的 Prompt 存在記事本裡有什麼不一樣？差別其實很大。
Prompt 是一次性的，講完就散了，下次要用得重打一次，而且每個人打的版本都不太一樣。Skill 是一份存在檔案系統裡的文件，可以進 Git 版控、可以跟著專案發給整個團隊、可以在改壞的時候回溯上一版，甚至可以寫測試去驗證它有沒有照規矩做事。
最關鍵的一點是「誰決定何時使用」：Prompt 要你自己記得貼上去，Skill 是 AI 自己判斷現在該不該用。所以我們說，不是 Prompt，是 Skill —— 你不是在下一次指令，你是在把自己的方法封裝成一個能被重複呼叫的能力。
-->

---

# 漸進式披露：Skill 省 token 的原理

Skill 不是「一次把所有知識塞給 AI」，而是**先給目錄，需要時才翻內頁**。

| 層 | 內容 | 何時載入 | 成本 |
| --- | --- | --- | --- |
| **第一層** 後設資料 | 只有 `name` + `description` | **常駐**上下文 | 每支數十 token |
| **第二層** 核心流程 | 完整的 `SKILL.md` 內文 | AI 判斷需要時才載入 | 一次性 |
| **第三層** 細則 | `references/` 底下的子文件 | 順著引用再往下讀 | 按需 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>就像查工具書：</b> 沒有人會把整本字典從第一頁讀到最後一頁，而是順著索引查詞條。裝了 50 支 Skill 也不會爆上下文，因為常駐的只有那份薄薄的目錄。
</div>

<!--
這一頁解釋了 Skill 最聰明的一個設計，叫漸進式披露。
想像你是新員工，公司不會把所有部門的操作手冊都堆到你桌上，而是先給你一份總目錄，你需要哪本再去取。Skill 就是這樣運作的。
第一層只有名稱跟描述，非常短，常駐在上下文裡，這是它「被看見」的成本。第二層是完整的 SKILL.md，只有 AI 判斷這次任務需要它時才會載入。第三層是 references 底下的子文件，順著引用再往下翻。
這就是為什麼你裝了五十支 Skill 也不會爆掉上下文——常駐的只有那份薄目錄。這也回頭解釋了我們前面說的：SKILL.md 要短，長文件放 references，因為第二層每次觸發都會整份讀進去。
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

每個 Skill 是一個資料夾，裡面必須有一份 `SKILL.md`，開頭要有 YAML frontmatter。

| 欄位 | 必填 | 說明 |
| --- | --- | --- |
| `name` | 必填 | 1–64 字元，小寫英數字加連字號 |
| `description` | 必填 | 說明用途，AI 靠這段判斷何時該用 |
| `license` | 選填 | 授權條款 |
| `metadata` | 選填 | 自訂的字串對照表 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>資料夾名稱要跟 name 一致：</b> 例如 <code>name: caveman</code>，資料夾就必須叫 <code>caveman</code>，兩者對不上會抓不到。
</div>

<!--
一份 Skill 其實就是一個資料夾加一份 Markdown 文件。
最重要的兩個欄位是 name 跟 description：name 是這個 Skill 的代號，會拿來對照資料夾名稱；description 則決定了 AI「什麼時候該想到用這個 Skill」，寫得越具體，AI 判斷得越準。
特別提醒一個常見的錯誤：資料夾名稱一定要跟 frontmatter 裡的 name 完全一樣，opencode 是靠這個對照才找得到，如果 description 沒寫，這個 Skill 甚至不會被 AI 看見。
-->

---

# description 是「路由條件」，不是功能介紹

它是常駐上下文中唯一被看見的東西，**決定這支 Skill 會不會在對的時機被叫出來**。

| 寫法 | 例子 | 結果 |
| --- | --- | --- |
| ❌ 功能介紹 | 「協助處理後端相關工作」 | 太寬泛，任何後端任務都觸發，路由失準 |
| ✅ 路由條件 | 「使用者說『我要開 PR』或要求送出前檢查時觸發」 | 邊界明確，該觸發才觸發 |
| ✅ 加上反例 | 「僅用於送出前檢查；**不**用於解讀既有 PR 的內容」 | 大幅減少誤觸發 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>一句話原則：</b>「何時該用我」比「我能做什麼」重要得多。description 要夠短（它常駐、每輪都佔 token），但必須寫得像路由條件。
</div>

<!--
這一頁是整堂課裡最容易被輕忽、卻最決定成敗的一件事。
回到剛才的漸進式披露：第一層常駐上下文的東西只有 name 跟 description，也就是說，AI 判斷「要不要用這支 Skill」時，手上唯一的資訊就是這一句話。所以它不是拿來介紹功能的，它是路由條件。
看這張表。寫「協助處理後端相關工作」，等於任何跟後端沾邊的任務都可能觸發，路由一定失準。改成「使用者說我要開 PR 時觸發」，邊界就清楚了。
更進階的做法是加反例：明講「不用於解讀既有 PR 的內容」，這一句可以擋掉大量誤觸發。
記住那句原則：「何時該用我」比「我能做什麼」重要得多。同時它還要夠短，因為它是常駐的，每一輪都在花你的 token。
-->

---

# SKILL.md — 範例

以本堂課全程示範用的「caveman」壓縮語氣 Skill 為例：

```yaml
---
name: caveman
description: 超壓縮溝通模式，砍掉贅字但保留完整技術內容，
  可省下約 75% token。支援 lite/full/ultra 三種強度。
  使用者說「講話省一點」「caveman 模式」或打 /caveman 時觸發。
metadata:
  intensity: lite | full | ultra
---
## 我會做的事
- 去掉冠詞、贅字、客套話，句子縮成片語
- 依 intensity 分級調整壓縮程度
- 遇到安全警告、需使用者確認的動作，自動切回正常語氣

## 使用時機
使用者要求省 token、講話簡短，或輸入 /caveman 時使用。
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
| 專案層級 | `.opencode/skills/<name>/SKILL.md` |
| 全域層級 | `~/.config/opencode/skills/<name>/SKILL.md` |
| 相容 Claude 專案層級 | `.claude/skills/<name>/SKILL.md` |
| 相容 Claude 全域層級 | `~/.claude/skills/<name>/SKILL.md` |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>安裝就是「放對資料夾」：</b> Skill 沒有專屬安裝指令，只要把資料夾放進上述任一路徑，opencode 下次啟動就會自動偵測到。
</div>

<!--
Skill 的「安裝」其實非常單純：不需要下載或執行任何安裝指令，只要把整個資料夾放進 opencode 會搜尋的路徑，它就自動生效了。
專案層級放在 .opencode/skills 底下，適合只給這個專案用、要進版本控制跟團隊共用的 Skill。
全域層級放在使用者家目錄的 .config/opencode/skills 底下，這樣不管開哪個專案都能用，適合個人習慣的通用技能。
opencode 也貼心相容了 Claude 的 skills 資料夾格式，如果你原本就有 Claude 的 Skill，直接放同一個位置兩邊都能吃到。
-->

---

# 補充：Plugin 提供的 Skill

有些 Skill 不是自己寫的資料夾，而是「隨 Plugin 一起安裝」，名稱會變成 `plugin:skill` 格式，例如我們示範用的 `caveman:caveman`。

| 類型 | 名稱格式 | 安裝方式 |
| --- | --- | --- |
| 本機 Skill | `skill-name` | 手動放資料夾 |
| Plugin Skill | `plugin-name:skill-name` | 安裝該 plugin，Skill 隨之附帶 |

<!--
除了自己手寫資料夾，還有一種常見情況：Skill 是跟著一整包 Plugin 一起裝進來的，這時候名稱前面會多一段 plugin 名稱當前綴，用冒號隔開，像 caveman:caveman 就是「caveman 這個 plugin 裡，名叫 caveman 的 skill」。
這種情況不用自己手動放資料夾，只要裝了對應的 plugin，裡面的 Skill 就自動可以用，等於買了一整組工具箱，裡面已經附好工具，不用自己一支一支組裝。
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

opencode 會透過內建的 `skill` 工具自動判斷並載入合適的 Skill，不用手動指名也能觸發。

```text
使用者：講話幫我省一點，開 caveman full 模式

AI（載入 caveman skill 後）：
Bug 在 auth middleware。Token 過期檢查用了 `<` 該用 `<=`。修正：
```

<!--
這一頁我們實際看一次 caveman skill 被觸發的樣子。
使用者只要說出「講話省一點」「caveman 模式」這類字眼，opencode 就會自己比對所有已安裝 Skill 的 description，發現 caveman 這份符合，自動載入內文的壓縮規則，之後的回覆就會照這套規則變成短句、去贅字，但技術內容完全不打折。
整個過程使用者完全不用打開 SKILL.md 看內容，這就是 Skill「隨插即用」的價值。
-->

---

# 權限設定

有些 Skill 比較敏感、或還在測試階段，可以用權限規則限制何時能自動套用。

```json
{
  "permission": {
    "skill": {
      "*": "allow",
      "caveman*": "ask",
      "internal-*": "deny"
    }
  }
}
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>完全關閉 Skill 功能：</b> 若某個 agent 完全不想用到任何 Skill，可在該 agent 設定裡加上 <code>"skill": false</code>。
</div>

<!--
如果團隊裡有些 Skill 比較敏感，不想讓它隨便自動執行，可以用 permission 規則管理：allow 代表直接放行，deny 代表完全隱藏不讓 AI 看到，ask 則是每次使用前都要先問過你。
像範例這樣，把 caveman 開頭的設成 ask，等於「AI 想切換成省話語氣前，要先問過我同不同意」，而內部測試用的 internal- 開頭則直接 deny，其餘全部 allow。
-->

---

# 用 CLI 確認已安裝的 Skill

安裝完想確認 opencode 有沒有抓到，可以用指令查看。

```bash
# 啟動 opencode 後，在對話框輸入
/skills
```

<!--
裝完 Skill 心裡毛毛的、不確定有沒有生效，很正常，這裡教大家一個檢查方式：直接在對話框打 /skills，opencode 會列出目前偵測到的所有 Skill，caveman 這種 plugin 提供的也會列在裡面，看到它就代表裝成功了。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 5. Production-grade Skill
# 一支「能上線」的 Skill

<!--
到這裡，我們已經能做出一支「可以動」的 Skill 了。但可以動，跟能不能放心交給團隊每天用，是兩回事。
-->

---

# 一支 Production-grade Skill 知道五件事

從「寫得出來」到「敢放上線」，差的就是這五個問題有沒有寫清楚。

| # | 它知道 | 沒寫清楚會發生什麼 |
| --- | --- | --- |
| ① | **什麼情況該啟動** | 該用時沒用，不該用時亂用 |
| ② | **先讀什麼** | 憑印象亂做，忽略團隊既有規範 |
| ③ | **照什麼流程做** | 每次步驟都不一樣，結果無法預期 |
| ④ | **哪些事情不能亂來** | 越權改動、刪錯檔案、跳過驗證 |
| ⑤ | **做完以後怎麼檢查** | 沒做完也說做完了，品質無法驗收 |

<!--
這五件事是整堂課最值錢的一頁，建議大家拍下來。
第一，什麼情況該啟動：這決定 AI 會不會在對的時機想到你這支 Skill。第二，先讀什麼：告訴它動手前要先看哪份規範、哪張對照表，不要憑印象亂做。第三，照什麼流程做：把步驟寫死，一二三四，每次都一樣。
第四，哪些事情不能亂來：這是「禁則」，比流程更重要，例如不准跳過測試、不准動 CI 設定、不准直接推 main。很多人只寫該做什麼，忘了寫不准做什麼，出事都是出在這裡。
第五，做完以後怎麼檢查：給它一組驗收條件，讓它自己確認有沒有真的做完，而不是嘴上說完成了。
五件事都寫齊，這支 Skill 才叫 production-grade。
-->

---

# 五要素怎麼寫進 SKILL.md

把上一頁的五個問題，直接對應成 `SKILL.md` 裡的五個區塊。

```yaml
---
name: pr-checklist
description: 開 PR 前的自我檢查流程；使用者說「我要開 PR」時觸發   # ① 何時啟動
---
## 開始前先讀 → references/commit-convention.md                # ② 先讀什麼
## 執行流程 → 1. 跑 lint  2. 跑測試  3. 檢查 commit message     # ③ 流程
## 禁止事項 → 不得跳過測試、不得改動 CI 設定、不得直接推 main    # ④ 禁則
## 完成後檢查 → 三項全通過才回報完成，任一失敗須列出原因         # ⑤ 驗收
```

<!--
看到沒有，五個要素其實就是五個標題，不需要多高深的技巧，重點是「有沒有寫」。
description 承擔第一個要素，決定觸發時機；接著開始前先讀，指向 references 裡的長文件；執行流程把步驟列死；禁止事項用「不得」開頭，講得越白話越好；完成後檢查給出可以判斷真假的驗收條件。
大家回去改自己的 Skill 時，就照這五個標題一項一項補，補完你會發現 AI 的表現穩定非常多。
-->

---

# 完整的 Skill 資料夾結構

Skill 不只有一份 `SKILL.md`，成熟的 Skill 會把資源、範例、工具、測試都放進來。

```text
pr-checklist/
├── SKILL.md      # 觸發條件、流程、禁則、驗收（五要素）
├── references/   # 需要時才讀的長文件：規範、對照表、術語
├── assets/       # 產出用的樣板、設定檔、圖檔
├── examples/     # 好與壞的範例，讓 AI 對齊品味
├── scripts/      # 可執行工具：validator、pack_skill.py
└── tests/        # 觸發測試與 regression 案例
```

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>為什麼要拆開：</b> <code>SKILL.md</code> 每次觸發都會被讀進去，要短；長文件放 <code>references/</code>，AI 需要時才去翻，省 token 也更好維護。
</div>

<!--
很多人卡在「我知道要做 Skill，但不知道資料夾裡該放什麼」，這一頁就是答案。
SKILL.md 是入口，要精簡，因為它每次被觸發都會整份讀進上下文。真正落落長的規範文件放在 references 底下，SKILL.md 只寫一句「需要時去讀 references/xxx.md」，AI 有需要才會翻開來看，這樣既省 token，改規範時也只要動一個檔案。
assets 放樣板跟設定檔，examples 放好例子跟壞例子，這對品質的幫助超乎想像，因為 AI 很擅長模仿範例。scripts 放可以實際執行的工具，tests 放測試案例。
不用一次做到滿，但心裡要知道這張地圖長什麼樣子。
-->

---

# 內容怎麼寫：一份 Skill 的四個區塊

不是背景知識，也不是一次成功對話的摘要，而是**讓新人照著就能做對**的指令。

| 區塊 | 寫什麼 |
| --- | --- |
| **角色與讀者** | 這份 Skill 服務誰、面向什麼任務、輸出要達到什麼標準 |
| **核心原則** | 只留三到五條最重要的判斷，關鍵原則配**正例與反例** |
| **禁止清單** | 高頻錯誤、越權動作、容易誤解的表達，同時寫清**合法例外** |
| **參考資料** | 術語表、樣板、範文與更詳細的子文件 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>規則的寫法公式：</b> 作用域 + 動作 + 例外 + 驗證方式。避免把所有可能情況堆成一張越來越長的禁用詞表。
</div>

<!--
上一頁講的是資料夾結構，這一頁講內文到底怎麼寫。這四個區塊來自提示工程師寶玉的《圖解 Skill》。
角色與讀者，先講清楚這份 Skill 服務誰、輸出標準是什麼。核心原則只留三到五條，貪多沒有用，而且關鍵原則一定要配正例跟反例——AI 非常擅長模仿範例，這比抽象描述有效得多。
禁止清單記高頻錯誤跟越權動作，但要注意一件事：同時要寫清楚合法的例外，否則它會過度保守。參考資料放術語表、樣板、範文。
最後那個公式很好用：作用域加動作加例外加驗證方式。很多人寫 Skill 會寫成一張越來越長的禁用詞表，那個方向是錯的，規則要有結構。
-->

---

# 怎麼從零寫出第一版：三步法

以「寫作風格 Skill」為例，**從你已有的成品反推規則**，而不是憑空想。

| 步驟 | 做什麼 |
| --- | --- |
| **1. 餵範例** | 挑三到五篇你最滿意的原創文章，讓 AI 歸納用詞、句式、段落結構、語氣 |
| **2. 跑真實任務** | 用產出的初版 Skill 處理一篇新文章，你再**逐句改稿** |
| **3. 回填差異** | 原文與改稿的差異最有資訊量：哪些詞被刪、哪些長句被拆，整理回 Skill |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>為什麼有效：</b> 「改稿前後的 diff」比抽象地說「寫得更自然一點」有資訊量得多。初版只要二十行左右就夠，重點是能跑真實任務並持續迭代。
</div>

<!--
很多人卡在「我知道要寫 Skill，但我的方法在我腦袋裡，寫不出來」。這個三步法就是解法：不要憑空想，從你已經有的成品反推。
第一步，挑三到五篇你最滿意的作品，讓 AI 幫你歸納：用詞習慣、句式、段落結構、語氣。它會生出一份大概二十行的初版。
第二步，用這份初版去處理一篇真實的新任務，然後你自己逐句改稿。
第三步是關鍵：原文跟你改稿之間的差異，資訊量非常大——哪些詞被你刪掉、哪些長句被你拆開、哪裡需要補事實。把這些反覆出現的改動整理回 Skill。
這比你坐在那裡想「我的風格是什麼」有效一百倍。初版短沒關係，能跑、能迭代才重要。
-->

---

# 該做成 Skill，還是做成專用工具？

兩種形態各有適用場景，用三個維度判斷。

| 維度 | 偏向**專用工具** | 偏向 **Skill + 通用執行器** |
| --- | --- | --- |
| **參數複雜度** | 巢狀物件、多欄位交叉驗證 | 參數簡單，用 CLI 傳參就夠 |
| **變更頻率** | 穩定的底層操作 | 常變的流程（改一段文字 vs 改程式碼＋測試＋部署） |
| **模型能力** | 較弱的模型，需要結構化 schema 引導 | 強模型可用自然語言 Skill 覆蓋更多場景 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>高風險操作永遠做成專用工具：</b> 付款、刪資料、寄信、生產部署——需要精細權限控制與稽核軌跡，不該交給「自然語言描述 + 通用執行器」。
</div>

<!--
這一頁回答一個實務問題：一個能力，我到底該寫成 Skill，還是該做成一個專用工具或 MCP？
三個維度。參數複雜度：如果要傳巢狀物件、多個欄位還要交叉驗證，專用工具的 schema 能更好地引導模型正確傳參；參數簡單的話，Skill 裡寫一行 CLI 指令就夠了。
變更頻率：常變的東西用 Skill，因為改一段文字，比改程式碼、改測試、重新部署輕鬆太多了。
模型能力：強模型可以靠自然語言 Skill 覆蓋很多場景，弱一點的模型比較需要結構化的 schema 牽著走。
最後那個警告請一定記住：付款、刪資料、寄信、部署這類高風險操作，永遠做成專用工具，因為你需要精細的權限控制跟稽核軌跡。
-->

---

# 安全：Skill 本身就是一個注入面

Skill 的本質是**「把外部內容當作指令載入」的制度化形式**。

| 風險 | 說明 |
| --- | --- |
| 第三方 Skill 夾帶惡意指令 | 效果比網頁裡的隱藏文字**更直接**——它本來就是被當指令讀的 |
| 來源不明的 Skill | 等同於執行一段你沒讀過的程式碼 |
| 靜默更新 | 今天安全，不代表更新後仍然安全 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>安裝前一定要讀過內容</b>，就像審查將要執行的程式碼一樣。這也是前面 <code>permission.skill</code> 設成 <code>ask</code> 或 <code>deny</code> 的真正價值所在。
</div>

<!--
講完怎麼寫，一定要補這一頁，因為它常被忽略。
Skill 的本質是什麼？是「把外部檔案的內容，當作指令載入到上下文」的制度化形式。所以它天生就是一個提示注入的入口，而且比網頁裡藏的隱藏文字更直接——網頁內容至少還是「資料」，Skill 內容本來就是被當「指令」讀的。
所以三個風險：第三方 Skill 可能夾帶惡意指令；來源不明的 Skill 等於執行一段你沒讀過的程式碼；就算今天安全，靜默更新後也可能變質。
結論很簡單：安裝前一定要把內容讀過一遍，用審查程式碼的標準去審查它。這也回頭解釋了我們前面講的 permission 設定——把不確定的 Skill 設成 ask，不是龜毛，是真的有必要。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 6. Skill 的進化五層級
# 你在哪一層？

<!--
接下來這一段，幫大家定位自己現在做的 Skill 在哪個階段，以及下一步往哪走。
-->

---

# Skill 的進化五層級

| 層級 | 名稱 | 內容物 | 你能做到什麼 |
| --- | --- | --- | --- |
| **L1** | Prompt Skill | 一份 `SKILL.md` | 把指令寫下來，可以用 |
| **L2** | Structured Skill | input / workflow / output / checks | 流程固定，輸出可預期 |
| **L3** | Resource Skill | references / assets / examples | 有資料與範例，品質對齊 |
| **L4** | Executable Skill | scripts / validator | 不只描述，還能執行工具 |
| **L5** | Production & Governance | tests / package / regression / 版本 / 凍結 | 穩定、可測、可管理 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>大多數人停在 L1–L2：</b> 從「可以用」到「穩定、可測、可管理」中間還差很多層，只有少數人做到 L5。
</div>

<!--
這張表是今天的地圖，大家可以誠實對照一下自己現在在哪一層。
L1 最常見，就是把一段指令寫成 SKILL.md，能動，但每次結果不太一樣。L2 開始有結構，明確定義輸入、流程、輸出、檢查點，結果就穩定多了，我們前面教的五要素大概就落在這裡。
L3 開始加資源，把規範文件、樣板、好壞範例放進來，AI 有東西可以參照，品質才會對齊團隊的品味。L4 再往上，Skill 不只是「描述怎麼做」，而是附帶可執行的腳本，例如一支 validator 直接幫你檢查格式對不對。
L5 是最少人到的一層，有測試、有錯誤處理、可以打包散佈、有 regression 保護、有版本規則跟凍結契約。今天不會全部展開，但我要讓大家看到方向：你現在做的東西，上面還有四層。
-->

---

# L5 到底多做了什麼？

| 項目 | 意義 |
| --- | --- |
| tests | 觸發測試 + 行為測試：該不該被叫、叫了有沒有做對事 |
| error handling | 前提不成立、工具失敗時該怎麼回報，而不是硬做下去 |
| package | 可打包散佈，別人裝上去就能用 |
| regression | 改版後，確認舊功能沒有被弄壞 |
| conventions | 團隊共用的命名、目錄與寫法規範 |
| 版本規則 | 語意化版本，改什麼要升哪一碼 |
| 凍結契約（FROZEN） | 對外行為鎖定，不得任意變更 |
| creator / owner | 誰負責維護、誰有權改版與驗收 |

<!--
這一頁把 L5 拆開來看，大家會發現，這些名詞其實軟體工程都有，只是套用到 Skill 上。
測試分兩種：觸發測試看它該不該被叫出來，行為測試看它被叫出來以後有沒有做對事。錯誤處理很容易被忽略，例如它要跑的測試指令根本不存在時，正確做法是停下來回報，而不是自己想辦法硬做。
package 決定別人能不能裝來用，regression 保護你改版不會弄壞舊功能，conventions 讓二十支 Skill 長得像同一個團隊寫的。版本規則跟凍結契約待會兒單獨講。
最後一項最容易被跳過但最重要：creator，也就是這支 Skill 是誰的責任。沒有負責人的 Skill，壞掉了也沒人修。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 7. 從一支到一整套
# Skill Engineering & Governance

<!--
做出一支 Skill 只是第一層，往後還有一整個工程世界。
-->

---

# 做出一支 Skill 只是第一層

當你手上有 20 支、50 支、100 支 Skill，會遇到的是**管理問題**，不是寫作問題。

| 你遲早會遇到的問題 | 對應的工程解法 |
| --- | --- |
| 誰負責判斷它到底做得好不好？ | **Benchmark**：訂出可量測的評分標準 |
| 誰規定一支 Skill 該符合什麼標準？ | **Conventions + validator**：格式自動檢查 |
| 100 支 Skill，誰在管理版本？ | **版本規則 + 凍結契約（FROZEN）** |
| 誰決定現在到底該執行哪一支？ | **觸發測試 + 多 Skill 衝突仲裁** |
| 出了問題，哪一個環節壞掉？ | **執行追蹤 + regression 測試** |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>一支 Skill 的後面，是一整套工程架構。</b>
</div>

<!--
這一頁是整堂課的視野頁。當你只有一支 Skill 的時候，這些問題都不存在，你自己知道它好不好用就好。
但一旦數量上來，五個問題會一個一個冒出來：誰說它做得好？標準是誰訂的？一百支的版本誰管？同時有三支都符合的時候聽誰的？出事了要怎麼知道是哪一環壞掉？
這些都不是「把 SKILL.md 寫好一點」能解決的，需要的是工程手段：benchmark、validator、版本規則、觸發測試、執行追蹤。
所以我常說，一支 Skill 的後面，是一整套工程架構。接下來幾頁，我們一項一項稍微摸一下。
-->

---

# 觸發測試與 Benchmark

| 測什麼 | 回答的問題 | 做法 |
| --- | --- | --- |
| **觸發測試** | 該不該被觸發？ | 準備「該觸發」與「不該觸發」的句子，逐一驗證 |
| **Benchmark** | 真的做得好嗎？ | 訂評分項目與及格線，同一組任務跑分比較 |

```text
[應觸發]   我要開 PR 了，幫我檢查一下        → pr-checklist ✅
[應觸發]   送出前先跑一下該跑的東西          → pr-checklist ✅
[不應觸發] 幫我看這個 PR 的內容在做什麼      → 不載入 ❌
```

<!--
觸發測試的做法很土法煉鋼，但非常有效：準備兩組句子，一組是使用者可能怎麼講、應該要觸發的，一組是聽起來很像但其實不該觸發的，然後一句一句丟給 AI 看它有沒有載入這支 Skill。
像下面這個例子，前兩句都該觸發 pr-checklist，第三句雖然也有 PR 兩個字，但使用者只是想了解內容，不該把檢查流程叫出來。這種「假陽性」最惱人，會讓 AI 一直做多餘的事。
Benchmark 則是回答另一個問題：它做得好不好？光「有觸發」不代表做得對，你要訂出評分項目，例如漏檢查幾項、格式對不對，然後每次改版都跑同一組任務比分數，這樣「變好了」才是有憑據的說法，不是感覺。
-->

---

# Regression 與多 Skill 衝突

| 情境 | 風險 | 處理原則 |
| --- | --- | --- |
| **改版** | 新增規則把舊行為弄壞 | 舊案例留成 regression，每次改版重跑 |
| **多支同時符合** | 兩支 Skill 講相反的話 | description 界線寫清楚、訂優先序 |
| **Skill 互相呼叫** | 責任邊界模糊、難以追查 | 一支只做一件事，組合交給上層決定 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>衝突時到底聽誰？</b> 與其讓 AI 自己猜，不如在 <code>SKILL.md</code> 裡直接寫明：「若同時符合 X Skill，以本 Skill 的禁則為準。」
</div>

<!--
Regression 的概念跟寫程式一模一樣：你改版是為了修 A 問題，結果不小心把本來好好的 B 功能弄壞了。做法是把每個修過的問題都留成一個測試案例，之後每次改版都重跑一次。
多 Skill 組合是數量上來以後最頭痛的問題。你有一支「講話省一點」的 caveman，又有一支「回報要寫完整檢查結果」的 pr-checklist，這兩支同時被觸發的時候，AI 到底要講短還是講完整？
最務實的解法不是期待 AI 自己判斷，而是在 SKILL.md 裡把仲裁規則直接寫死：遇到誰要讓誰、誰的禁則優先。
還有一個原則要記住：一支 Skill 只做一件事。一支包山包海的 Skill 出問題時，你完全不知道是哪一段壞掉。
-->

---

# 版本規則與凍結契約（FROZEN）

當 Skill 被別人依賴時，「隨手改一下」就是破壞性變更。

```yaml
metadata:
  version: 2.3.0            # 語意化版本：破壞性 / 新功能 / 修正
  status: FROZEN            # DRAFT（開發中）/ ACTIVE（可用）/ FROZEN（凍結）
  owner: platform-team      # 誰負責維護與驗收
  frozen_contract:          # 凍結後不得再改的對外行為
    - 輸出格式固定為 checklist
    - 觸發語意不得擴張
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>FROZEN 不是不能改：</b> 是「要改就要升版本、要重跑 regression、要通知使用者」，而不是偷偷改掉。
</div>

<!--
這裡的 metadata 欄位是團隊自己訂的慣例，opencode 不會強制檢查，但這正是治理的起點：把約定寫進檔案裡，大家才有共同語言。
version 用語意化版本，破壞性變更升第一碼、加新功能升第二碼、修 bug 升第三碼。status 標示它處於什麼階段，DRAFT 表示還在玩、ACTIVE 表示可以用、FROZEN 表示已經有人依賴了。
frozen_contract 是最關鍵的一段：它把「對外承諾」明文列出來，例如輸出格式固定是 checklist，觸發語意不能再擴張。凍結不代表這支 Skill 從此不能動，而是要改就得走完整流程：升版本、重跑 regression、通知使用者，不能今天心血來潮就偷偷改掉，因為別人的流程正靠著它。
-->

---

# 工具鏈：讓流程可以被執行

把慣例寫成腳本放進 `scripts/`，Skill 的生命週期就能自動化。

```bash
python scripts/bootstrap.py pr-checklist    # 產生標準資料夾骨架
python scripts/validator.py pr-checklist    # 檢查 frontmatter 與必要區塊
python scripts/run_tests.py pr-checklist    # 觸發測試 + regression
python scripts/pack_skill.py pr-checklist   # 打包成可散佈的 skill 套件
```

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>這些不是 opencode 內建指令：</b> 是團隊自行撰寫的工具腳本。重點在「慣例只要能被執行，就不會有人忘記照做」。
</div>

<!--
先說清楚，這四支腳本不是 opencode 內建的指令，是團隊自己寫的工具，我把它們列出來是要傳達一個觀念：慣例如果只寫在文件裡，一定會有人忘記照做；但只要能被一行指令執行，它就會真的被執行。
bootstrap 幫你長出標準骨架，新人要寫 Skill 不用問資料夾該放什麼。validator 檢查 frontmatter 有沒有缺欄位、五個必要區塊有沒有寫齊，這一支投報率最高，可以直接掛進 CI。
run_tests 跑觸發測試跟 regression，pack_skill 把整包打包成可以散佈的格式，讓別人裝了就能用。
這就是 L4 到 L5 的差別：你的 Skill 不只被閱讀，還有工具在幫你守住品質。
-->

---

# 從 0 到 Production 的開發流程

| 階段 | 做什麼 | 產出 |
| --- | --- | --- |
| 1. 挖流程 | 把你腦中的做法逐步寫下來 | 草稿 |
| 2. 補五要素 | 觸發、先讀、流程、禁則、驗收 | `SKILL.md` |
| 3. 加資源 | 規範文件、樣板、好壞範例 | `references/` `examples/` |
| 4. 加工具 | validator、可執行腳本 | `scripts/` |
| 5. 加測試 | 觸發測試、benchmark、regression | `tests/` |
| 6. 上版控 | 版本、owner、凍結契約 | `metadata` + Git |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>不必一次做到 L5：</b> 先讓它能用（1–2），有人真的在用了再往上加（3–6）。過早工程化跟完全不工程化一樣浪費。
</div>

<!--
最後把整套流程收成一張表，這就是從 0 開始做出屬於你自己的 Skill 的完整路徑。
第一步是挖流程，把你腦袋裡那套做法逐步寫下來，這一步不需要任何技術，但最花力氣，因為很多人其實沒意識到自己的方法是什麼。第二步補上五要素，這時候它就從 L1 進到 L2。
第三步加資源、第四步加工具、第五步加測試、第六步上版控，一層一層往上疊。
但我要強調最後這個提醒：不必一次做到 L5。先讓它能用，等到真的有人在用、真的開始出問題了，再往上加。過早工程化跟完全不工程化一樣浪費時間。
另外，你不需要是工程師才能做這件事，你需要的是願意動手，把自己的方法寫下來、讓各平台的 AI 都能重複使用。
-->

---
layout: default
---

# 練習：體驗並仿寫 caveman Skill
### 任務說明

先在 opencode 對話框打字觸發本堂課示範用的 caveman skill，感受壓縮語氣的效果；接著仿照它的結構，寫一份 `.opencode/skills/pr-checklist/SKILL.md`，內容是「開 PR 前的自我檢查清單」（跑哪些測試指令、commit message 格式規則）。

---
layout: default
---

# 練習：體驗並仿寫 caveman Skill
### 解題提示

1. 先打「講話幫我省一點」或「caveman 模式」，觀察 AI 回覆語氣的變化，確認 Skill 真的被載入了
2. 仿照 caveman 的 frontmatter 寫法，想清楚 `description` 要怎麼寫，才能讓 AI 準確判斷「開 PR 前」該用你的 Skill
3. 內文條列出檢查項目，例如跑 lint、跑測試、確認 commit message 格式
4. 存到 `.opencode/skills/pr-checklist/SKILL.md`，資料夾名稱記得跟 `name` 對齊
5. 啟動 opencode，跟它說「我要開 PR 了，幫我檢查一下」，看它有沒有自動套用這份 Skill

<!--
這個練習分兩步：先體驗別人寫好的 caveman skill 長什麼感覺，再仿照同樣的結構自己寫一份。
重點提示一下，description 是關鍵中的關鍵，寫得夠具體，AI 才會在對的時機把這份 Skill 抽出來用，就跟 caveman 那份寫「使用者說想省 token 時觸發」一樣明確。
-->

---
layout: default
---

# 練習：把 pr-checklist 升級到 L3
### 任務說明

把上一題寫好的 `pr-checklist` 從 L1 往上推：補齊「五要素」五個區塊（何時啟動、先讀什麼、流程、禁則、驗收），再新增 `references/commit-convention.md` 放 commit message 規範、`examples/` 放一組好與壞的範例，最後為它設計三句觸發測試句子。

---
layout: default
---

# 練習：把 pr-checklist 升級到 L3
### 解題提示

1. 五個區塊各用一個 `##` 標題開頭，**禁則一定要寫**，用「不得⋯⋯」的句型最清楚
2. `SKILL.md` 只留一句「commit 格式請參考 `references/commit-convention.md`」，長規範搬到 references，別塞進主文件
3. `examples/` 放一個合格與一個不合格的 commit message，讓 AI 有得對照
4. 觸發測試準備三句：兩句「應觸發」、一句「聽起來很像但不該觸發」（例如：幫我看這個 PR 在做什麼）
5. 三句逐一實測，若判斷錯誤，回頭修 `description` 的界線描述，而不是改內文

<!--
這一題是把剛才那份陽春的 pr-checklist，實際往上推一層，體驗一下 L1 到 L3 的差別。
重點有三個。第一，五個區塊裡最容易漏的是禁則，大家一定要寫，而且用「不得」開頭，講得越白話越好。第二，長規範一定要搬到 references，SKILL.md 只留一句指向它，這樣主文件才不會越長越肥。
第三個重點是最後那兩步：寫完一定要實測三句話，特別是那句「聽起來很像但不該觸發」的。如果它判斷錯了，記得回頭修的是 description，因為觸發時機是由 description 決定的，改內文沒有用，這是很多人一開始會踩的坑。
-->

---
layout: end
---

# 課程結束
### 下一步：認識 MCP，讓 opencode 接上外部工具與服務！

<!--
今天我們從入門一路走到工程化：Skill 是什麼、怎麼寫、放在哪裡、怎麼控制權限，接著看了一支 production-grade Skill 該知道的五件事、Skill 的五個進化層級，還有數量多起來以後會遇到的測試、Benchmark、版本與治理問題。
記得那句話：不是 Prompt，是 Skill —— 把你的能力變成可以被 AI 重複使用的能力。回去先挑一件你每週都要做的事，把它寫成一支 Skill，能用就好，之後再慢慢往上加層。
下一堂課，我們要介紹 MCP，讓 opencode 除了寫程式，還能連上 GitHub、資料庫等外部工具！
-->
