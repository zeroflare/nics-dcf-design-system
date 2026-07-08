# @nics/design-tokens

DCF 存取節點平台設計系統的**唯一來源 ( single source of truth )**。所有平台引用這裡的 token,改一次全部生效。

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
