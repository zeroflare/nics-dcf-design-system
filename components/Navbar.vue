<script setup lang="ts">
/**
 * Navbar — 導覽列殼層 ( 2026.7.27 拍板新增，跨站共用 )
 * 實體住在 nics-dcf-design-system，消費端由 @nics/design-tokens/components/Navbar.vue 引用
 *
 * 業主要求各站視覺一致，做法是把「長相」收進殼層：56px 高、白底、底部 stroke 分隔線、
 * 左中右三塊排版與間距，各站帶入自己的內容，差異只留在內容層，不留在樣式層。
 * 定位 ( fixed / sticky ) 交由消費端外層決定，本元件只管高度與內部排版；
 * 行動版漢堡選單 / Drawer 亦不屬本殼層範圍，由各站自行處理。
 *
 * 根元素刻意用 div 不用 header：HTML5 規格禁止 header 巢狀在另一個 header 底下，
 * 消費端通常會用自己的 <header class="fixed ..."> 包住本元件，header 語意角色留給
 * 消費端外層 ( 一個頁面只該有一個導覽 header 地標 )。
 *
 * 2026.7.28 拍板:左側 brand、中間選單、右側 actions 三塊包在一起設最大寬 1280px
 * ( 與頁面內容容器同一套 max-w-7xl px-4 md:px-6，桌面斷點置中，其他斷點隨容器寬度自然收縮 )，
 * 中間選單改為在此最大寬容器內置中，不再貼齊左側 brand。
 *
 * 插槽：
 *   - brand   左側標誌區 ( logo + 站名，兩行文字建議 text-body2 font-bold text-fg-primary-default
 *             主名、-mt-1 text-caption text-fg-secondary-default 副名 )
 *   - default 中間選單，置中顯示，建議搭配 NavbarItem 使用
 *   - actions 右側動作區 ( 連線狀態、使用者頭像、按鈕等 )
 * 消費端只覆寫佈局類 ( 寬高、間距 )，不得覆寫顏色 / 字級 / 圓角。
 */
</script>

<template>
  <div class="h-14 border-b border-stroke bg-bg-container">
    <div class="mx-auto flex h-full max-w-7xl items-center gap-4 px-4 md:px-6">
      <div class="flex h-full min-w-0 shrink-0 items-center">
        <slot name="brand" />
      </div>
      <nav class="flex h-full flex-1 items-center justify-center text-body2 text-fg-secondary-default">
        <slot />
      </nav>
      <div class="flex h-full min-w-0 shrink-0 items-center gap-6">
        <slot name="actions" />
      </div>
    </div>
  </div>
</template>
