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

### 1. 安裝 token 套件

```bash
bun add github:zeroflare/nics-dcf-design-system
```

在全站 CSS 進入點 ( 如 `src/style.css` ) 引入：

```css
@import 'tailwindcss';
@import '@nics/design-tokens/tokens.css';
```

### 2. shadcn 橋接變數指向 token

shadcn 元件透過橋接變數吃色，安裝 shadcn 後在 `style.css` 的 `:root` 把以下變數改指 token ( 這是拍板值，不是建議值 )：

```css
--primary: var(--color-brand-500);        /* 主操作色,甲方需求書 #0d9488 */
--primary-foreground: #ffffff;
--destructive: var(--color-red-500);      /* 危險色,甲方需求書 #ef4444 */
--ring: var(--color-brand-300);           /* focus 光暈,經 ring/50 半透明渲染 */
--border: var(--color-stroke);            /* 邊框,單一 stroke = neutral-200 */
```

### 3. 安裝認可的 shadcn 元件並重放有意修改

用 `npx shadcn-vue add <name>` 安裝需要的元件。**認可清單**：button、input、switch、checkbox、select、textarea、dialog、drawer、popover、command、dropdown-menu、tooltip、badge、skeleton、progress、sonner、card、table、separator、pagination、collapsible。

**明確不使用**：alert、scroll-area、tabs ( 膠囊式；分頁採底線式規格，見 component.html Tabs 章 )。

安裝後重放三個拍板過的 vendor 修改 ( 修改處都要留「有意的 vendor 修改」註解 )：

| 檔案 | 修改 |
|---|---|
| `ui/dialog/DialogTitle.vue` | class 的 `'text-lg leading-none font-semibold'` → `'text-subtitle'` ( Subtitle 18 / 700 / 1.3 ) |
| `ui/card/Card.vue` | class 的 `rounded-xl` → `rounded-md` ( 卡片圓角一律 md 8px ) |
| `ui/progress/Progress.vue` | 新增 `indicatorClass` prop 並以 `cn()` 併入 indicator class，供指定語意 solid 填色 |

### 4. 自訂元件直接從套件 import

```ts
import Tag from '@nics/design-tokens/components/Tag.vue'
import ConfirmDialog from '@nics/design-tokens/components/ConfirmDialog.vue'
```

**依賴契約**：自訂元件內部以 `@/components/ui/*` 引用 shadcn 元件 ( 如 ConfirmDialog 用到 dialog、button )，因此消費端必須：(1) `@` 別名指向 `src`；(2) 已完成步驟 3 安裝對應的 shadcn 元件。

### 5. 遵守元件規格

實作任何 UI 前先讀 `component.html` 對應元件的「已拍板規則」：

- 覆寫僅限佈局類 ( 寬高、間距 )，不得覆寫樣式 ( 色彩、圓角、字級 )
- 小尺寸有既定慣例 ( Input 為 `class="h-8 text-xs"`、SelectTrigger 用 `size="sm"`、icon 按鈕用 `size="icon-sm"` )，不得用 class 硬寫尺寸
- 需要新的共用元件或樣式 pattern：**先回本 repo 立章定案 ( 讀碼 → 現況 → 提案 → 設計負責人拍板 )，元件實體放本 repo 的 `components/`**，不得建立在各專案內
