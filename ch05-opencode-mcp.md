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
到目前為止，opencode 能做的事都在你的電腦裡：讀檔案、改程式、執行指令。
但寫程式的工作，很多是在電腦以外的地方，像是 GitHub 上的 issue、pull request。
這堂課要介紹 MCP，它可以幫 AI 接上外部的服務。我們會用 GitHub 官方的 MCP server 當例子，讓 opencode 直接讀寫你在 GitHub 上的專案。
-->

---
layout: default
---

# Outline

- **0. 情境：AI 看不到你的 GitHub** — 這堂課要解決的問題
- **1. 什麼是 MCP** — Model Context Protocol 的概念
- **2. 準備 GitHub 權杖** — 建立 Personal Access Token
- **3. 設定 GitHub MCP Server** — remote 與 local 兩種方式
- **4. 使用與權限管理** — 在對話中使用、控制 AI 能做什麼

<!--
今天分五段。
先看一個實際的情境，了解為什麼需要 MCP。
接著認識 MCP 是什麼。
然後準備連線 GitHub 需要的權杖，再實際把 GitHub MCP server 接進 opencode。
最後學怎麼在對話中使用，以及怎麼控制 AI 能做哪些事，這點非常重要。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 0. 情境：AI 看不到你的 GitHub
# Your Project Lives Online Too

<!--
先來看一個大家很快就會遇到的情境。
-->

---

# 情境：AI 只看得到你的電腦

你的練習專案放在 GitHub 上，用 issue 記錄待辦事項和 bug。但 opencode 預設只看得到你電腦裡的檔案。

| 想請 AI 做的事 | 沒有 MCP 時 |
| --- | --- |
| 「幫我看一下還有哪些 issue 沒處理」 | 要自己打開網頁，一個一個複製貼上 |
| 「照 issue #3 的描述修改程式」 | 要自己把 issue 內容貼給 AI |
| 「把剛剛發現的 bug 開成一個 issue」 | 要自己到網頁上填表單 |
| 「這個 PR 的留言說了什麼？」 | 要自己去看、自己整理 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>接上 GitHub MCP 之後：</b> 上面這些事，都可以直接在 opencode 裡用一句話完成。
</div>

<!--
很多人會把專案放在 GitHub 上，用 issue 當作待辦清單，記錄要做的功能、發現的 bug。
但 opencode 預設只看得到你電腦裡的檔案，看不到 GitHub 上的東西。
所以你想請 AI 照某個 issue 修改程式，就要自己打開網頁、複製 issue 內容、貼給 AI，很麻煩。
接上 GitHub 的 MCP server 之後，AI 就能自己去讀 issue、開 issue、看 PR，你只要一句話就好。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 1. 什麼是 MCP？
# Model Context Protocol

<!--
接著來認識 MCP。
-->

---

# 什麼是 MCP？

「一套讓 AI 連接外部工具與資料的標準協定」，讓 opencode 除了讀寫本地檔案，還能使用 GitHub、資料庫、瀏覽器等外部服務。

| 沒有 MCP | 有 MCP |
| --- | --- |
| AI 只能看得到你電腦裡的檔案 | AI 能讀寫 GitHub 上的 issue、PR |
| 每接一個新服務都要客製化開發 | 用同一套協定接上任何相容服務 |
| 你要當 AI 和外部服務之間的「傳話筒」 | AI 自己去查、自己去做 |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>比喻：</b> MCP 就像電腦的 USB 接口，不管你插滑鼠、鍵盤還是隨身碟，規格統一。AI 只要「插上」對應的 MCP server，就能立刻多一項新能力。
</div>

<!--
MCP 的全名是 Model Context Protocol，它是一套標準，規定 AI 要怎麼跟外部的工具、資料溝通。
最好的比喻是 USB：不管插的是滑鼠還是隨身碟，規格都一樣，插上去就能用。
很多服務都推出了自己的 MCP server，GitHub 就是其中之一。AI 只要接上 GitHub 的 MCP server，就會多出一組操作 GitHub 的工具，像是列出 issue、建立 issue、開 PR。
有了 MCP，你就不用再當 AI 跟 GitHub 之間的傳話筒了。
-->

---

# MCP Server 的兩種類型

| 類型 | 運作方式 | 優點 | 缺點 |
| --- | --- | --- | --- |
| `remote` | 連到架在雲端的 server，透過網址連線 | 不用在電腦上安裝任何東西 | 需要網路，要看服務商有沒有提供 |
| `local` | 在你的電腦上執行一支程式 | 可以自己控制、離線工具也能用 | 要安裝執行環境（例如 Docker、Python） |

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>GitHub 兩種都有：</b> GitHub 官方提供雲端的 remote server，也提供可以在本機執行的版本。新手建議用 remote，設定最簡單。
</div>

<!--
MCP server 分成兩種。
remote 是架在雲端的，opencode 透過網址連過去，你的電腦不用安裝任何東西。
local 是在你的電腦上執行一支程式，好處是可以自己控制，缺點是要先安裝執行環境。
GitHub 官方兩種都有提供，等一下兩種設定都會看到，但新手建議直接用 remote，最簡單。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 2. 準備 GitHub 權杖
# Personal Access Token

<!--
要讓 AI 操作你的 GitHub，得先給它一把鑰匙。
-->

---

# 建立 Personal Access Token

權杖（token）就是讓 AI 代替你操作 GitHub 的鑰匙。建議用 **Fine-grained token**，只開放需要的權限。

| 步驟 | 說明 |
| --- | --- |
| 1. 進入設定 | GitHub 右上角頭像 → `Settings` → `Developer settings` |
| 2. 建立權杖 | `Personal access tokens` → `Fine-grained tokens` → `Generate new token` |
| 3. 設定到期日 | 練習用設 30 天就好 |
| 4. 選擇 repo | `Only select repositories`，**只選你的練習專案** |
| 5. 設定權限 | `Contents`：Read-only；`Issues`、`Pull requests`：Read and write |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>權限開越小越安全：</b> 不要選 All repositories，也不要開用不到的權限。權杖只會顯示一次，記得馬上存好。
</div>

<!--
要讓 AI 代替你操作 GitHub，需要一把鑰匙，也就是 Personal Access Token，簡稱權杖。
GitHub 的權杖有兩種，建議用 Fine-grained token，它可以精細地設定這把鑰匙能開哪些門。
建立的路徑是：右上角頭像、Settings、最下面的 Developer settings，再選 Fine-grained tokens。
有三個設定特別重要。第一，到期日，練習用的設 30 天就好，到期自動失效比較安全。第二，repo 的範圍，請只選你的練習專案，不要選全部。第三，權限，這門課需要讀取程式碼、讀寫 issue 跟 PR，其他都不要開。
權限開越小，萬一 AI 做錯事，影響的範圍也越小。
-->

---

# 把權杖存進環境變數

權杖不要寫在設定檔裡，用環境變數保存，設定檔再用 `{env:變數名稱}` 讀取。

```bash
# macOS / Linux（寫進 ~/.zshrc 或 ~/.bashrc，之後每次開終端機都會有）
export GITHUB_PAT="你的權杖"

# Windows PowerShell（設定為使用者環境變數，重開終端機後生效）
[Environment]::SetEnvironmentVariable("GITHUB_PAT", "你的權杖", "User")
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>為什麼不直接寫進設定檔：</b> <code>opencode.jsonc</code> 通常會跟專案一起上傳到 GitHub，權杖寫在裡面等於公開給所有人。
</div>

<!--
拿到權杖之後，不要直接貼進 opencode 的設定檔。因為設定檔通常會跟專案一起上傳到 GitHub，等於把鑰匙公開給全世界。
正確的做法是存進環境變數。Mac 跟 Linux 把 export 這一行寫進 shell 的設定檔，像 .zshrc；Windows 用 PowerShell 這一行設定成使用者環境變數。
設定好之後要重開終端機才會生效。等一下在 opencode 的設定檔裡，我們會用 {env:GITHUB_PAT} 這個寫法去讀它。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 3. 設定 GitHub MCP Server
# Remote & Local

<!--
鑰匙準備好了，開始把 GitHub MCP server 接進 opencode。
-->

---

# Remote 設定（建議）

連到 GitHub 官方架設的 MCP server，電腦上不用安裝任何東西。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "github": {
        "type": "remote",
        "url": "https://api.githubcopilot.com/mcp/",
        "headers": { "Authorization": "Bearer {env:GITHUB_PAT}" },
        "oauth": false  // 用權杖登入，關閉 OAuth 流程
      }
    }
  }
}
```

<!--
這是最推薦的設定方式，寫在 opencode.jsonc 裡。
所有 MCP server 都放在 mcp 底下的 servers 裡，這裡取名叫 github。
type 是 remote，url 是 GitHub 官方 MCP server 的網址。
headers 裡的 Authorization 會帶上你的權杖，{env:GITHUB_PAT} 代表從環境變數讀取，所以設定檔裡看不到真正的權杖。
最後的 oauth: false，是因為 remote server 預設會走瀏覽器登入的 OAuth 流程，我們已經用權杖了，所以把它關掉。
-->

---

# 縮小範圍：Toolsets 與唯讀模式

GitHub MCP 的工具非常多，可以用 header 只開需要的部分。

```jsonc
"github": {
  "type": "remote",
  "url": "https://api.githubcopilot.com/mcp/",
  "oauth": false,
  "headers": {
    "Authorization": "Bearer {env:GITHUB_PAT}",
    // 只開這三組工具
    "X-MCP-Toolsets": "repos,issues,pull_requests",
    // 唯讀：只能看、不能改
    "X-MCP-Readonly": "true"
  }
}
```

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>MCP 會佔用上下文：</b> 每個工具的說明都會佔用 AI 的對話額度。工具開越多，AI 越容易選錯、也越快把上下文用完。
</div>

<!--
GitHub 的 MCP server 工具非常多，有管 Actions 的、管安全性的、管專案看板的。全部打開的話，AI 光是讀工具說明就要花很多上下文。
所以可以用兩個 header 縮小範圍。X-MCP-Toolsets 指定要開哪幾組工具，這門課用 repos、issues、pull_requests 三組就夠了。
X-MCP-Readonly 設成 true，就變成唯讀模式，AI 只能看、不能改任何東西。剛開始練習的時候，建議先用唯讀模式熟悉，確定沒問題再拿掉。
-->

---

# Local 設定（進階）

在自己電腦上用 Docker 執行 GitHub 官方的 MCP server。

```jsonc
"github": {
  "type": "local",
  "command": [
    "docker", "run", "-i", "--rm",
    "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
    "ghcr.io/github/github-mcp-server"
  ],
  "environment": {
    "GITHUB_PERSONAL_ACCESS_TOKEN": "{env:GITHUB_PAT}"
  }
}
```

<div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 text-gray-700 text-sm text-left">
💡 <b>什麼時候用 local：</b> 公司網路連不到 GitHub 的雲端 MCP，或想固定使用某個版本時。需要先安裝 Docker，新手用 remote 就好。
</div>

<!--
這是 local 的設定方式，給大家參考。
type 是 local，command 是啟動指令，這裡是用 Docker 執行 GitHub 官方發布的 MCP server 映像檔。
environment 把我們的權杖傳給這支程式，名稱 GITHUB_PERSONAL_ACCESS_TOKEN 是 GitHub MCP server 規定的。
local 的好處是可以自己控制版本，或是在 remote 連不到的環境使用，但需要先安裝 Docker。對新手來說，用 remote 就足夠了。
-->

---

# 用指令新增與確認

不想手寫設定檔，也可以用 `opencode mcp add` 新增，它會自動寫進設定檔。

```bash
# 新增 GitHub 的 remote server
opencode mcp add github --url https://api.githubcopilot.com/mcp/ \
  --header 'Authorization=Bearer {env:GITHUB_PAT}'

# 查看目前接了哪些 server
opencode mcp list
```

| 檢查方式 | 做法 |
| --- | --- |
| 終端機 | `opencode mcp list`，看 `github` 有沒有出現 |
| opencode 介面內 | 輸入 `/mcps` 查看連線狀態 |
| 直接問 AI | 「你現在有哪些 GitHub 相關的工具可以用？」 |

<!--
除了手寫設定檔，也可以用 opencode mcp add 指令新增，--url 接網址，--header 接權杖的 header，執行後它會自動寫進設定檔。
新增完要確認有沒有接成功，有三種方法：在終端機跑 opencode mcp list；在 opencode 介面裡輸入 /mcps；或者直接問 AI 現在有哪些 GitHub 的工具可以用。
如果連不上，最常見的原因是環境變數沒設好，記得設定完要重開終端機。
-->

---
layout: section
class: flex flex-col justify-center items-center text-center
---

# 4. 使用與權限管理
# Use & Control Access

<!--
接上之後，來看怎麼用，以及怎麼控制 AI 能做什麼。
-->

---

# 在對話中使用 GitHub 工具

接上之後不用記工具名稱，直接用自然語言描述，AI 會自己判斷要用哪個工具。

```text
幫我列出 my-todo-app 這個 repo 還沒關閉的 issue，
依照難易度排序，告訴我建議先處理哪一個
```

```text
（先按 Shift+Tab 切到 plan）
讀一下 issue #3 的內容，告訴我你打算怎麼修改程式
```

```text
我剛發現按下刪除鍵後畫面沒有更新，
幫我開一個 issue 記錄這個 bug，標題和描述用繁體中文
```

<!--
接上 GitHub MCP 之後，用法跟平常聊天一樣，直接說你要做什麼就好。
第一個例子，請 AI 列出還沒處理的 issue，還請它依難易度排序、給建議，這對新手決定先做什麼很有幫助。
第二個例子，請 AI 讀一個 issue，然後搭配前面學過的 plan 模式，先看它打算怎麼改。
第三個例子，請 AI 幫你開一個 issue 記錄 bug，以後就不用自己到網頁上填表單了。
-->

---

# 權限管理：讓 AI 先問過你

原則是「**讀取放行、寫入先問、危險禁止**」。每條規則的格式：`{ "action": "github_issue_write", "resource": "*", "effect": "ask" }`，放進 `"permissions": [ ... ]` 陣列。

| 順序 | `action`（server 名稱_工具名稱） | `effect` | 意思 |
| --- | --- | --- | --- |
| 1 | `github_*` | `allow` | 先全部放行（讀 issue、看程式碼…） |
| 2 | `github_issue_write` | `ask` | 開／改 issue 前先問你 |
| 3 | `github_create_pull_request` | `ask` | 開 PR 前先問你 |
| 4 | `github_merge_pull_request` | `deny` | 禁止合併 PR |

<div class="mt-4 p-3 bg-yellow-50 border-l-4 border-yellow-400 text-gray-700 text-sm text-left">
⚠️ <b>後面的規則優先：</b> 所以「全部放行」要寫在最前面。工具名稱可能隨版本調整，可以先問 AI 它看到的工具名稱。
</div>

<!--
AI 能直接操作 GitHub，代表它也可能做錯事，例如不小心合併了不該合併的 PR，所以權限管理一定要設。
跟前一堂課的 Skill 一樣，用 permissions 規則來管理。MCP 工具的 action 名稱，是 server 名稱、底線、工具名稱，我們的 server 叫 github，所以就是 github_ 開頭。
建議的原則是：讀取放行、寫入先問、危險禁止。範例中，先把所有 github 工具放行，再把寫 issue 跟開 PR 設成 ask，每次都要你同意；合併 PR 直接 deny，AI 完全不能做。如果還有其他危險的工具，例如刪除檔案，也可以照同樣方式加上 deny。
注意規則是由上往下比對、後面的優先，所以「全部放行」要寫在最前面。
另外，GitHub MCP 的工具名稱偶爾會調整，如果規則沒生效，可以先問 AI 它看到的工具名稱是什麼。
-->

---
layout: default
---

# 練習：用 issue 驅動你的開發

### 任務說明

1. 在 GitHub 建立一個練習 repo（例如第 6 章做的待辦清單），手動開 2～3 個 issue 寫下想加的功能
2. 建立一個**只能存取這個 repo** 的 Fine-grained token，存進環境變數
3. 用 remote 方式接上 GitHub MCP，先開啟 `X-MCP-Readonly` 唯讀模式
4. 請 AI 列出所有 issue 並建議處理順序，再挑一個 issue 用 `plan` 模式討論做法

<!--
這個練習會用到前面好幾章的內容。
先在 GitHub 建一個練習 repo，可以用第 6 章做的待辦清單，自己開幾個 issue，寫下想加的功能。
接著建立權杖、接上 GitHub MCP，一開始先用唯讀模式，確定 AI 只能看、不能改。
最後請 AI 讀 issue、排優先順序，再挑一個用 plan 模式討論做法。
-->

---
layout: default
---

# 練習：用 issue 驅動你的開發

### 解題提示

1. 權杖的 repo 範圍選 `Only select repositories`，權限只開 Contents、Issues、Pull requests
2. 設定完環境變數要**重開終端機**，再用 `opencode mcp list` 確認 `github` 有連上
3. 唯讀模式下請 AI 開 issue，觀察它會怎麼回應（應該會說沒有權限）
4. 熟悉之後拿掉唯讀模式、加上權限規則，再請 AI 幫你開一個 issue，看它會不會先問你

<!--
提示第三點很有趣：在唯讀模式下故意請 AI 開 issue，它應該會告訴你沒有權限，這可以讓你親眼確認唯讀模式真的有效。
第四點則是熟悉之後，拿掉唯讀、加上權限規則，再請它開 issue，這時候 opencode 應該會先跳出來問你同不同意。
兩種保護方式都體驗過，你就知道怎麼在方便跟安全之間取得平衡。
-->

---
layout: end
---

# 課程結束
### 下一步：學會怎麼跟 AI 說話，開始真正的 Vibe Coding！

<!--
今天我們用 GitHub 當例子認識了 MCP：它是什麼、remote 跟 local 兩種 server 怎麼設定、怎麼建立權杖、怎麼在對話中使用，以及怎麼用權限規則控制 AI 能做的事。
回顧一下整個系列，我們從安裝 opencode 開始，接上 Mistral 拿到 AI 大腦，學會用 Skill 記住固定流程，最後用 MCP 接上 GitHub 這個外部工具箱。
現在，opencode 已經是一位裝備齊全、隨時待命的 AI 工程師夥伴了。
下一堂課開始，我們要學怎麼跟這位夥伴合作：把需求講清楚、先規劃再動手，正式進入 vibe coding！
-->
