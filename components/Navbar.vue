<script setup lang="ts">
/**
 * Navbar — 導覽列殼層 ( 2026.7.27 拍板新增，跨站共用 )
 * 實體住在 nics-dcf-design-system，消費端由 @nics/design-tokens/components/Navbar.vue 引用
 *
 * 業主要求各站視覺一致，做法是把「長相」收進殼層：56px 高、白底、底部 stroke 分隔線、
 * 左右兩塊排版與間距，各站帶入自己的內容，差異只留在內容層，不留在樣式層。
 * 定位 ( fixed / sticky ) 交由消費端外層決定，本元件只管高度與內部排版；
 * 行動版漢堡選單 / Drawer 亦不屬本殼層範圍，由各站自行處理。
 *
 * 插槽：
 *   - brand   左側標誌區 ( logo + 站名，兩行文字建議 text-body2 font-bold text-fg-primary-default
 *             主名、-mt-1 text-caption text-fg-secondary-default 副名 )
 *   - default 中間選單，建議搭配 NavbarItem 使用
 *   - actions 右側動作區 ( 連線狀態、使用者頭像、按鈕等 )
 * 消費端只覆寫佈局類 ( 寬高、間距 )，不得覆寫顏色 / 字級 / 圓角。
 */
</script>

<template>
  <header
    class="flex h-14 items-center justify-between gap-4 border-b border-stroke bg-bg-container px-4 md:px-6"
  >
    <div class="flex h-full min-w-0 shrink-0 items-center gap-6">
      <slot name="brand" />
      <nav class="flex h-full items-center text-body2 text-fg-secondary-default">
        <slot />
      </nav>
    </div>
    <div class="flex h-full min-w-0 items-center gap-6">
      <slot name="actions" />
    </div>
  </header>
</template>
