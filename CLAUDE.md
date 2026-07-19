# 這個 repo 是什麼

`@nics/design-tokens`,資安院 (NICS) DCF 存取節點平台的 **design token 與共用元件唯一來源 (single source of truth)**。所有消費端專案 (目前是 `nics-dcf-access-demo`,未來可能還有其他平台) 都透過安裝這個套件取得:

- `tokens.css` — 顏色、字級、間距、圓角、陰影等 token
- `components/` — 設計系統自訂元件 ( Tag、ConfirmDialog 等 ),消費端以 `@nics/design-tokens/components/*.vue` 引用

改一次這裡,全部專案生效。對外規格書網站:`index.html` ( 進站首頁 )、`foundation.html` ( 設計規範 )、`component.html` ( 元件規格與盤點 ),部署在 Cloudflare 給甲方查看。新專案接入流程見 `INTEGRATION.md`。

# 三個 repo 的關係

1. **`dcf-access-playground`** — 設計決策的討論與拍板現場。所有 token 的新增、調整,都是先在那邊跟負責的產品設計師一章一章討論定案,不是在這裡自己決定
2. **`nics-dcf-design-system`** (本 repo) — 把拍板定案的內容落地成 `tokens.css`,並用根目錄的 `index.html` 對外發布成正式設計系統網站 (部署在 Cloudflare,給甲方查看)
3. **`nics-dcf-access-demo`** — 實際消費 token 的產品程式碼,透過 `@nics/design-tokens` 引用本 repo

# 你 (Claude Code) 在這個 repo 工作時務必遵守

1. **`tokens.css` 是唯一權威來源**。不要為了單一頁面的需求,在消費端專案裡另外 override 或發明新的顏色/間距/圓角數值,任何新 token 需求都要回頭到 `dcf-access-playground` 討論拍板
2. **規格書網頁必須跟程式碼保持同步**。修改 `tokens.css` 或 `components/` 的任何內容,都要同步更新 `foundation.html` / `component.html` 對應章節,避免文件與程式碼脫節
3. **不要自行發明新的 token 命名規則**。目前架構是三層:Base ( 純色階,`--color-*` )→ Function ( 語意層,只引用 Base )→ Component ( 元件層,只引用 Function )。新增 token 要照這個階層放,不要跳層直接讓元件引用 Base
4. **如果不確定某個數值是否已經拍板定案,不要用猜的**。可以先去 `dcf-access-playground` 的 `memory/memory.md` 或本 repo 的規格書網頁查證,真的不確定就直接問負責的產品設計師,不要自行推論

# 元件與樣式變更流程 ( 2026.7.20 拍板 )

1. **元件來源三層制** ( component.html 每個元件都有標籤 ):
   - **shadcn 原生**:實體放各消費端專案的 `components/ui/` ( CLI 慣例 ),原始碼不修改
   - **shadcn + 有意修改**:同上,但安裝後要重放拍板過的修改 ( 目前 3 個:DialogTitle 套 text-subtitle、Card 圓角 md、Progress 加 indicatorClass ),修改處必須留「有意的 vendor 修改」註解
   - **自訂**:實體放**本 repo 的 `components/`**,消費端由套件 import,不得複製進專案
2. **新增共用元件或樣式 pattern 的流程**:先在 `component.html` 立章、走「讀碼 → 整理現況 → 提案 → 設計負責人拍板」四步,拍板後實體放本 repo `components/`,再由消費端引用。**嚴禁直接在消費端專案建立共用元件**
3. **修改既有元件樣式前**,先讀 `component.html` 該元件的「已拍板規則」,不得牴觸;要牴觸就回 `dcf-access-playground` 重新拍板
4. **消費端的樣式覆寫僅限佈局類** ( 寬高、間距 ),色彩、圓角、字級不得覆寫

# 設計網站時須遵守的事 ( index.html 對外發布內容適用 )

1. 使用繁體中文、台灣用語,不能出現簡體中文和支語
2. 中文句子中使用到括號,要遵守使用半形括號並前後空格的規則,例如:存取節點 (Access Node)
3. 數字要使用千分號,例如:1,000
4. 嚴禁在句子中間使用 —— 破折號來銜接或引出結論
5. 如果有任何不清楚或有疑問的地方,先詢問再實作,不要自行推論腦補
