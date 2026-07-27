# 新專案接入指南

> 適用對象：所有資料匯流平台相關網站的新專案 ( 及其協作的 Claude Code session )。
> 本設計系統是**唯一來源**：token、共用元件、元件規格都以本 repo 為準，不得在各專案自行發明。
> 網頁版規格書：`foundation.html` ( 設計規範 )、`component.html` ( 元件，開頭有本指南的章節版 )。

## 元件來源三層制

| 層級 | 說明 | 實體位置 |
|---|---|---|
| shadcn 原生 | 原始碼與官方一致，未修改 | 各專案自己的 `src/components/ui/` ( CLI 安裝 ) |
| shadcn + 有意修改 | 官方為底，帶本設計系統拍板的修改 ( 檔內有註解 ) | 各專案 `src/components/ui/`，安裝後**重放修改** ( 見步驟 3 ) |
| 自訂 | 設計系統自己的產物 | 本 repo `components/`，由套件 import，**不得複製進專案** |

component.html 每個元件的標題旁都有來源標籤，一眼可辨。

## 接入步驟

### 0. 把 CLAUDE.md 模板放進新專案根目錄

這步讓之後所有 AI session 自動載入治理規則，**不做這步，後續 session 對設計系統一無所知**。把下面模板存成新專案根目錄的 `CLAUDE.md` ( 確認 .gitignore 沒有排除它 )，並依專案名調整：

```markdown
# 這個 repo 是什麼

[專案名],資料匯流平台相關網站之一,消費 `@nics/design-tokens` 的 token 與共用元件。

# 設計系統治理 ( 必守 )

1. 設計規範與元件的唯一來源是 `nics-dcf-design-system` repo:
   規格書為該 repo 的 foundation.html ( 設計規範 ) 與 component.html ( 元件規格 ),
   接入與維護流程為該 repo 的 INTEGRATION.md,動 UI 前先讀
2. 樣式一律用 token 的語意 class,不手寫 raw Tailwind 色階與數值
3. 共用元件不得建立在本 repo:新元件需求回 dcf-access-playground 與設計負責人拍板,
   實體放 nics-dcf-design-system 的 components/,本 repo 由套件 import
4. shadcn vendor 元件 ( components/ui/ ) 依 INTEGRATION.md 的認可清單安裝,
   有意修改需重放並留註解;明確不使用:alert、scroll-area、膠囊式 tabs
5. 元件覆寫僅限佈局類 ( 寬高、間距 ),色彩、圓角、字級不得覆寫
6. 不確定是否拍板過的數值,查規格書,不要用猜的

# 文字規則

1. 繁體中文、台灣用語,不能出現簡體中文和支語
2. 中文句子中使用半形括號並前後空格,例如:存取節點 (Access Node)
3. 數字使用千分號,例如:1,000
4. 嚴禁在句子中間使用 —— 破折號銜接或引出結論
5. 不清楚就先問再實作,不要自行推論腦補
```

### 1. 安裝 token 套件

```bash
bun add github:zeroflare/nics-dcf-design-system
```

在全站 CSS 進入點 ( 如 `src/style.css` ) 引入，並把套件的元件目錄加進 Tailwind 掃描來源 ( Tailwind 預設不掃 node_modules，漏這行自訂元件會沒有樣式 )：

```css
@import 'tailwindcss';
@import '@nics/design-tokens/tokens.css';
@source '../node_modules/@nics/design-tokens/components';
```

### 2. shadcn 橋接變數指向 token

shadcn 元件透過橋接變數吃色，安裝 shadcn 後在 `style.css` 的 `:root` 把以下變數改指 token ( 這是拍板值，不是建議值 )。2026.7.27 拍板：全部 shadcn 語意變數都要橋接，不留 shadcn 原生 oklch 預設值：

```css
--background: var(--color-bg-surface);            /* 頁面底色,淺灰 */
--foreground: var(--color-fg-primary-default);
--card: var(--color-bg-container);                 /* 卡片白底 */
--card-foreground: var(--color-fg-primary-default);
--popover: var(--color-bg-container);
--popover-foreground: var(--color-fg-primary-default);
--primary: var(--color-brand-500);                 /* 主操作色,甲方需求書 #0d9488 */
--primary-foreground: var(--color-fg-inverted-default);
--secondary: var(--color-bg-container-variant);
--secondary-foreground: var(--color-fg-primary-default);
--muted: var(--color-bg-container-variant);
--muted-foreground: var(--color-fg-secondary-default);
--accent: var(--color-bg-container-variant);
--accent-foreground: var(--color-fg-primary-default);
--destructive: var(--color-red-500);               /* 危險色,甲方需求書 #ef4444 */
--border: var(--color-stroke);                      /* 邊框,單一 stroke = neutral-200 */
--input: var(--color-stroke);
--ring: var(--color-brand-300);                     /* focus 光暈,經 ring/50 半透明渲染 */
```

**不橋接**：`.dark` 整塊、`--chart-1~5`、`--sidebar-*`。這些是 shadcn 鷹架的預設殘留，本設計系統目前沒有深色模式，也沒有任何消費站裝 chart / sidebar 元件，橋接了也沒有元件會吃到，留著純粹是 shadcn CLI 裝元件時一起帶出來的死碼，之後真的要做深色模式或裝 chart/sidebar 才需要處理。

### 3. 安裝認可的 shadcn 元件並重放有意修改

用 `npx shadcn-vue add <name>` 安裝需要的元件。**認可清單**：button、input、switch、checkbox、select、textarea、dialog、drawer、popover、command、dropdown-menu、tooltip、badge、progress、sonner、card、table、separator、pagination、collapsible。

**明確不使用**：alert、scroll-area、tabs ( 膠囊式；分頁採底線式規格，見 component.html Tabs 章 )、skeleton ( 載入骨架改用自訂 Shimmer，2026.7.27 退役，見 component.html Shimmer 章 )。

安裝後重放七個拍板過的 vendor 修改 ( 修改處都要留「有意的 vendor 修改」註解 )：

| 檔案 | 修改 |
|---|---|
| `ui/dialog/DialogTitle.vue` | class 的 `'text-lg leading-none font-semibold'` → `'text-subtitle'` ( Subtitle 18 / 700 / 1.3 ) |
| `ui/dialog/DialogContent.vue`<br>`ui/dialog/DialogScrollContent.vue` | class 的 `bg-background` → `bg-container` ( ds 白卡底;`--background` 橋接到 bg-surface 頁面底色淺灰,對話框應與卡片同為白底,不能沿用頁面底色 ) |
| `ui/drawer/DrawerContent.vue` | class 的 `bg-background` → `bg-container` ( ds 白卡底,理由同 Dialog；2026.7.24 拍板 ) |
| `ui/dialog/DialogOverlay.vue`<br>`ui/dialog/DialogScrollContent.vue`<br>`ui/drawer/DrawerOverlay.vue` | class 的 `bg-black/80` → `bg-black/60` ( 遮罩不透明度調淡,阿暖反饋 80% 太暗,全站遮罩統一；2026.7.28 拍板 ) |
| `ui/card/Card.vue` | ① class 的 `rounded-xl` → `rounded-md` ( 卡片圓角一律 md 8px )<br>② 移除 `border` ( 框線與 shadow-sm 都在做與背景分離,並存會疊出灰邊使陰影顯重;需要框線的區塊自行加 `border-stroke` ) |
| `ui/progress/Progress.vue` | 新增 `indicatorClass` prop 並以 `cn()` 併入 indicator class，供指定語意 solid 填色 |
| `ui/switch/Switch.vue` | thumb 的 `bg-background` → `bg-fg-inverted-default` ( 純白;`--background` 橋接到頁面底色淺灰,疊在實色軌道上會顯髒 ) |

### 4. 自訂元件直接從套件 import

```ts
import Tag from '@nics/design-tokens/components/Tag.vue'
import ConfirmDialog from '@nics/design-tokens/components/ConfirmDialog.vue'
import Shimmer from '@nics/design-tokens/components/Shimmer.vue' // 頁面級載入骨架 ( 掃光 )
import Avatar from '@nics/design-tokens/components/Avatar.vue' // 使用者頭像 ( 首字 + 主色漸層 )
import Navbar from '@nics/design-tokens/components/Navbar.vue' // 導覽列殼層,插槽 brand / 預設 / actions
import NavbarItem from '@nics/design-tokens/components/NavbarItem.vue' // 搭配 Navbar 的選單項
import Footer from '@nics/design-tokens/components/Footer.vue' // 頁尾殼層
```

**依賴契約**：自訂元件內部以 `@/components/ui/*` 引用 shadcn 元件 ( 如 ConfirmDialog 用到 dialog、button )，因此消費端必須：(1) `@` 別名指向 `src`；(2) 已完成步驟 3 安裝對應的 shadcn 元件。`NavbarItem` 額外依賴 `vue-router`( 所有消費站都是 vue-router SPA，視為既有前提，不需額外安裝 )。

### 5. 遵守元件規格

實作任何 UI 前先讀 `component.html` 對應元件的「已拍板規則」：

- 覆寫僅限佈局類 ( 寬高、間距 )，不得覆寫樣式 ( 色彩、圓角、字級 )
- 小尺寸有既定慣例 ( Input 為 `class="h-8 text-xs"`、SelectTrigger 用 `size="sm"`、icon 按鈕用 `size="icon-sm"` )，不得用 class 硬寫尺寸
- 需要新的共用元件或樣式 pattern：**先回本 repo 立章定案 ( 讀碼 → 現況 → 提案 → 設計負責人拍板 )，元件實體放本 repo 的 `components/`**，不得建立在各專案內
