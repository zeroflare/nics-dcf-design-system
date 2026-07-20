# @nics/design-tokens

DCF 存取節點平台設計系統的**唯一來源 ( single source of truth )**。所有平台引用這裡的 token 與共用元件,改一次全部生效。

> **📖 規格書網站 ( 不必 clone,直接開 ) → https://nics-dcf-design-system.zeroflare.workers.dev/**
> 設計規範 ( 顏色 / 文字 / 間距 / 圓角 / 陰影 ) 與元件規格 ( 每個元件的已拍板規則、用法、preview ) 都在上面。
> 寫樣式前想確認「這個顏色 / 字級 / 元件該用哪個 token」,查這裡最快。

> **🚀 新專案接入必讀 [INTEGRATION.md](./INTEGRATION.md)** — 包含 CLAUDE.md 模板、token 安裝、shadcn 元件清單與有意修改重放、自訂元件引用的完整步驟。AI 協作者 ( Claude Code 等 ) 進到本 repo 或任何消費端專案工作前,也請先讀這份。

## 使用方式 ( Tailwind v4 )

在產品的 `src/style.css`,於 `@import "tailwindcss"` 之後加入:

```css
@import "tailwindcss";
@import "@nics/design-tokens/tokens.css";
```

安裝 ( git dependency ):

```bash
bun add github:<org>/nics-dcf-design-system
```

## 三層架構

1. **Base** — 純色階 13 條 + 字體 / 字級 / 圓角 / 陰影,放 `@theme`,會生成 Tailwind utility ( 例如 `bg-brand-500`、`text-title1` )
2. **Function** — 語意層,只引用 Base ( 例如 `--fg-primary`、`--success-bg` ),元件以 `var()` 取用
3. **Component** — 元件層,目前先收 Tag ( 語意 5 種 + 分類 12 種 )

## 已定案內容

色彩三層、Typography ( 字體 / 字級 / 行高 / 字重 )、Spacing、Radius、Shadow。詳見 tokens.css 內註解。
