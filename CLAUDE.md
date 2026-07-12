# 這個 repo 是什麼

`@nics/design-tokens`,資安院 (NICS) DCF 存取節點平台的 **design token 唯一來源 (single source of truth)**。所有消費端專案 (目前是 `nics-dcf-access-demo`,未來可能還有其他平台) 都透過安裝這個套件、引用 `tokens.css` 來取得顏色、字級、間距、圓角、陰影等 token,改一次這裡,全部專案生效。

# 三個 repo 的關係

1. **`dcf-access-playground`** — 設計決策的討論與拍板現場。所有 token 的新增、調整,都是先在那邊跟負責的產品設計師一章一章討論定案,不是在這裡自己決定
2. **`nics-dcf-design-system`** (本 repo) — 把拍板定案的內容落地成 `tokens.css`,並用根目錄的 `index.html` 對外發布成正式設計系統網站 (部署在 Cloudflare,給甲方查看)
3. **`nics-dcf-access-demo`** — 實際消費 token 的產品程式碼,透過 `@nics/design-tokens` 引用本 repo

# 你 (Claude Code) 在這個 repo 工作時務必遵守

1. **`tokens.css` 是唯一權威來源**。不要為了單一頁面的需求,在消費端專案裡另外 override 或發明新的顏色/間距/圓角數值,任何新 token 需求都要回頭到 `dcf-access-playground` 討論拍板
2. **`index.html` 必須跟 `tokens.css` 保持同步**。修改 `tokens.css` 的任何 token 數值或新增/刪除項目,都要同步更新 `index.html` 裡對應的章節內容,避免文件與程式碼脫節
3. **不要自行發明新的 token 命名規則**。目前架構是三層:Base ( 純色階,`--color-*` )→ Function ( 語意層,只引用 Base )→ Component ( 元件層,只引用 Function,目前只有 Tag )。新增 token 要照這個階層放,不要跳層直接讓元件引用 Base
4. **如果不確定某個數值是否已經拍板定案,不要用猜的**。可以先去 `dcf-access-playground` 的 `memory/memory.md` 或 `index.html` 查證,真的不確定就直接問負責的產品設計師，不要自行推論

# 設計網站時須遵守的事 ( index.html 對外發布內容適用 )

1. 使用繁體中文、台灣用語,不能出現簡體中文和支語
2. 中文句子中使用到括號,要遵守使用半形括號並前後空格的規則,例如:存取節點 (Access Node)
3. 數字要使用千分號,例如:1,000
4. 嚴禁在句子中間使用 —— 破折號來銜接或引出結論
5. 如果有任何不清楚或有疑問的地方,先詢問再實作,不要自行推論腦補
