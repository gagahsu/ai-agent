---
theme: penguin
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
title: opencode + Mistral 的使用
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
    opencode + Mistral 的使用
  </h1>
  <div style="height: 4px; width: 320px; background: linear-gradient(90deg, #5eada0, #a7d9d0); border-radius: 2px; margin-bottom: 1.5rem;"></div>
  <p style="color: #4a7c7c; font-size: 1.15rem; font-style: italic;">
    「把鑰匙插進車子，正式發動 AI 引擎」
  </p>
  <Link to="home" style="color: #9dc4c4; font-size: 0.85rem; margin-top: 2rem; text-decoration: none; letter-spacing: 0.05em;">← 返回目錄</Link>
</div>

<!--
前兩堂課，我們裝好了 opencode，也申請了 Mistral 的 API key。
今天就要把兩者接起來，讓 opencode 真正開始運作。
學完這堂課，大家就能在終端機裡請 AI 幫忙寫程式了。
-->

---
layout: default
---

# Outline

- **1. 回顧** — opencode 與 Mistral 各自扮演的角色
- **2. 連接 Mistral** — 用 `/connect` 或環境變數
- **3. 設定預設模型** — `opencode.jsonc` 設定檔
- **4. 選擇與切換模型** — `/models` 實際操作與常見問題

<!--
今天分四段。
先快速回顧 opencode 跟 Mistral 的關係。
再實際把 API key 接進 opencode，有兩種方法。
接著學怎麼用設定檔指定預設模型，不用每次都重選。
最後學怎麼切換模型，以及遇到錯誤時怎麼排除。
-->

---

# 回顧：opencode 與 Mistral 的關係

「opencode 是操作介面，Mistral 是背後提供 AI 能力的供應商」，兩者是搭配關係，不是同一個東西。

| 角色 | 負責的事 |
| --- | --- |
| opencode | 終端機介面、讀寫檔案、執行指令 |
| Mistral | 提供實際思考與生成內容的 AI 模型 |
| API Key | 兩者之間的通行證 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>比喻：</b> opencode 是車子，Mistral 是引擎，API key 是車鑰匙。車子本身不會跑，要插上鑰匙、發動引擎才會動。
</div>

<!--
先快速回顧一下。
opencode 是我們操作的介面，它負責讀寫檔案、執行指令。
Mistral 是真正在思考、產生內容的 AI 模型。
API key 就是兩者之間的通行證，opencode 拿著這把鑰匙去呼叫 Mistral 的模型。
用車子來比喻：opencode 是車子，Mistral 是引擎，API key 是鑰匙。今天就是要把鑰匙插進去，把車子發動。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 連接 Mistral
# Connect Your Key

<!--
開始把 API key 接進 opencode。
-->

---

# 方法一：用 /connect 接上 Mistral（建議）

opencode 內建支援 Mistral，不需要透過其他轉接服務。

| 步驟 | 操作 |
| --- | --- |
| 1 | 啟動 opencode，輸入 `/connect` |
| 2 | 在供應商清單中搜尋並選擇 `Mistral` |
| 3 | 貼上上一堂課申請的 API key |
| 4 | 輸入 `/models`，選擇 `devstral-latest` |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>只要設定一次：</b> 用 <code>/connect</code> 設定的金鑰會存在你的電腦上（不在專案資料夾裡），之後開新專案不用重複設定，也不會不小心被上傳到 GitHub。
</div>

<!--
最簡單的方法是用 /connect。
啟動 opencode 之後輸入 /connect，會跳出一個供應商清單，搜尋 Mistral 選進去，把上一堂課存好的 API key 貼上就完成了。
接著輸入 /models，選 devstral-latest，這是 Mistral 專門為寫程式設計的模型。
用這個方法設定的金鑰會存在你電腦的使用者設定裡，不在專案資料夾裡，所以之後開其他專案都不用重新設定，也不用擔心金鑰被一起上傳到 GitHub。
-->

---

# 方法二：用環境變數

如果你習慣用環境變數，或想在腳本裡使用，可以設定 `MISTRAL_API_KEY`。

```bash
# macOS / Linux（只在這個終端機視窗有效）
export MISTRAL_API_KEY="你的金鑰"

# Windows PowerShell（只在這個視窗有效）
$env:MISTRAL_API_KEY="你的金鑰"

# 設定好之後啟動 opencode
opencode
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>新手建議用方法一：</b> 環境變數關掉終端機就會消失，要永久保存得寫進 shell 設定檔，步驟比較多。<code>/connect</code> 設定一次就好。
</div>

<!--
第二個方法是設定環境變數。opencode 啟動時會自動讀取 MISTRAL_API_KEY 這個環境變數。
Mac 跟 Linux 用 export，Windows PowerShell 用 $env: 的寫法。
不過要注意，這樣設定只在目前這個終端機視窗有效，關掉就沒了。要永久保存，得寫進 shell 的設定檔，對新手來說步驟比較多。
所以一般建議用第一個方法 /connect，設定一次就好。環境變數比較適合寫腳本或自動化的時候用。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 設定預設模型
# opencode.jsonc

<!--
接下來學怎麼用設定檔指定預設模型。
-->

---

# 用設定檔指定預設模型

在專案根目錄建立 `opencode.jsonc`，每次啟動就會自動使用指定的模型。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  // 預設使用的模型：格式是「供應商/模型 ID」
  "model": "mistral/devstral-latest"
}
```

| 設定檔位置 | 效果 |
| --- | --- |
| 專案根目錄 `opencode.jsonc` | 只有這個專案使用 |
| `~/.config/opencode/opencode.jsonc` | 所有專案的預設值 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>.jsonc 是什麼：</b> 就是可以寫註解（<code>//</code> 開頭）的 JSON。opencode 也接受一般的 <code>opencode.json</code>。
</div>

<!--
每次開 opencode 都要用 /models 選一次模型很麻煩，可以用設定檔指定預設模型。
在專案根目錄建立 opencode.jsonc，寫上 model 這個欄位。格式是「供應商斜線模型 ID」，所以 Mistral 的 Devstral 就是 mistral/devstral-latest。
如果希望所有專案都用同一個模型，可以放在使用者設定資料夾 ~/.config/opencode 裡面。
附帶一提，jsonc 就是可以寫註解的 JSON，雙斜線後面的文字是給人看的說明，opencode 會忽略它。
-->

---

# 備案：自訂供應商設定

如果你的 `/connect` 清單裡找不到 Mistral，可以把它當成「OpenAI 相容」的供應商手動設定。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "providers": {
    "mistral": {
      "name": "Mistral",
      "env": ["MISTRAL_API_KEY"],
      "package": "@opencode/ai/providers/openai-compatible",
      "settings": { "baseURL": "https://api.mistral.ai/v1" },
      "models": { "devstral-latest": { "name": "Devstral" } }
    }
  },
  "model": "mistral/devstral-latest"
}
```

<!--
正常情況下，用 /connect 就能找到 Mistral。
但如果你遇到清單裡沒有的狀況，例如 opencode 版本比較舊，可以用這個備案：把 Mistral 當成「OpenAI 相容」的供應商手動設定。
意思是 Mistral 的 API 跟 OpenAI 的 API 格式相同，所以 opencode 可以用同一套方式跟它溝通。
baseURL 是 Mistral API 的網址，env 告訴 opencode 去哪個環境變數找金鑰，models 列出要使用的模型。
這張投影片大家知道有這個方法就好，大部分情況用不到。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 選擇與切換模型
# Pick Your Model

<!--
最後來看怎麼選擇、切換模型，開始真正使用。
-->

---

# 切換模型與開始對話

模型設定好之後，用 `/models` 切換，然後直接打字對話即可開始使用。

```bash
# 啟動 opencode
opencode

# 對話框內查看並切換模型（也可以按 Ctrl+X 再按 M）
/models

# 選定模型後，直接輸入任務
幫我在 src/utils 底下新增一個 formatDate 函式，
輸入 Date 物件，回傳 yyyy-mm-dd 格式字串
```

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>什麼時候換模型：</b> 平常用 <code>devstral-latest</code> 寫程式；想問概念、請它解釋時可以換成 <code>mistral-medium-latest</code>；遇到很複雜的規劃再換 <code>mistral-large-latest</code>。
</div>

<!--
設定好之後，啟動 opencode，輸入 /models 就可以看到所有可用的模型，也可以用快捷鍵 Ctrl+X 再按 M。
選好模型後，直接在對話框打字描述你的任務就可以了，像這裡請它新增一個日期格式化的函式。
至於什麼時候換模型：平常寫程式用 devstral 就好；想問概念、請它解釋的時候，可以換成比較快的 mistral-medium；遇到很複雜、需要仔細規劃的任務，再換成 mistral-large。
-->

---

# 常見問題排除

| 錯誤狀況 | 可能原因 | 解決方法 |
| --- | --- | --- |
| `401 Unauthorized` | 金鑰貼錯，或剛建立還沒生效 | 重新複製金鑰，或等幾分鐘再試 |
| `429 Too Many Requests` | 碰到免費方案的用量上限 | 等一下再試，或升級 Scale 方案 |
| 找不到模型 | 模型 ID 打錯，或模型已被淘汰 | 用 `/models` 從清單選，不要手打 |
| 清單沒有 Mistral | opencode 版本較舊 | 更新 opencode，或使用自訂供應商設定 |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>401 和 429 最常見：</b> 401 是「鑰匙不對」，429 是「用太快了」。看到 429 不用緊張，休息一下就好。
</div>

<!--
最後整理幾個最常遇到的錯誤。
401 Unauthorized 代表鑰匙不對，通常是金鑰複製的時候少了幾個字，或是剛建立還沒生效，重新複製或等幾分鐘就好。
429 Too Many Requests 代表用太快了，免費方案的上限比較嚴格，很容易碰到，休息一下再試就好；如果常常遇到，可以考慮升級付費方案。
找不到模型，通常是模型 ID 打錯，建議用 /models 從清單裡選，不要自己手打。
如果清單裡根本沒有 Mistral，可能是 opencode 版本太舊，更新一下，或者用剛剛的備案設定。
-->

---
layout: default
---

# 練習：接上 Mistral，完成第一個任務

### 任務說明

1. 用 `/connect` 接上你的 Mistral API key
2. 在練習專案建立 `opencode.jsonc`，把預設模型設成 `mistral/devstral-latest`
3. 重新啟動 opencode，請 AI 做一個顯示「Hello, 我的名字」的 `index.html`
4. 用 `/models` 換成 `mistral-medium-latest`，請它解釋剛剛的程式

<!--
這個練習把今天學的全部串起來。
先用 /connect 接上金鑰，再用設定檔指定預設模型，接著請 AI 做一個最簡單的網頁。
最後換一個模型請它解釋程式，感受一下切換模型的流程。
-->

---
layout: default
---

# 練習：接上 Mistral，完成第一個任務

### 解題提示

1. `/connect` 清單很長，直接打字搜尋 `mistral` 比較快
2. 設定檔的模型格式是「供應商/模型 ID」，中間用 `/` 隔開
3. 重新啟動後，看畫面上顯示的模型名稱，確認設定檔有生效
4. 遇到 401 或 429 錯誤，對照「常見問題排除」那一頁

<!--
提示第三點很重要，重新啟動 opencode 後，畫面上會顯示目前使用的模型，看到 devstral 就代表設定檔有生效。
如果遇到錯誤，先別慌，回頭看常見問題排除那一頁，大部分都能自己解決。
-->

---
layout: end
---

# 課程結束
### 下一步：學會安裝與使用 Agent Skills，讓 opencode 更懂你的專案！

<!--
今天我們把 Mistral 的 API key 接進了 opencode，學了 /connect 跟環境變數兩種方法，用設定檔指定預設模型，也學會切換模型跟排除常見錯誤。
現在大家的 opencode 已經可以正式開始工作了。
下一堂課，我們要學 Agent Skills，教 AI 記住固定的工作流程，讓它更懂你的專案。
-->
