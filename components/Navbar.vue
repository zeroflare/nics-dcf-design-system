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
 * 2026.7.28 拍板:新增 fluid 佈局 prop。全寬工作頁 ( 如資料來源的桌面式分割版面 ) 內容本身撐滿
 * 整個視窗寬，此時 navbar 若仍鎖 1280 會像「戴小帽子」( 內容貼邊、logo/actions 內縮 )。
 * fluid=true 時拿掉 max-w-7xl 讓內層也撐滿，logo 貼左緣、actions 貼右緣，對齊底下全寬內容。
 * 中間選單中心點在 1280 置中與全寬兩種模式下數學上不變 ( 左右內縮量對消 )，故切頁時選單不位移、
 * 只有 logo / actions 往左右滑出，切換順滑。各站由路由決定何時傳入 ( demo 用 route.meta.fullWidth )。
 *
 * 插槽：
 *   - brand   左側標誌區 ( logo + 站名，兩行文字建議 text-body2 font-bold text-fg-primary-default
 *             主名、-mt-1 text-caption text-fg-secondary-default 副名 )
 *   - default 中間選單，置中顯示，建議搭配 NavbarItem 使用
 *   - actions 右側動作區 ( 連線狀態、使用者頭像、按鈕等 )
 * props：fluid ( 佈局用，是否撐滿全寬，預設 false = 鎖 1280 )
 * 消費端只覆寫佈局類 ( 寬高、間距、fluid )，不得覆寫顏色 / 字級 / 圓角。
 */
withDefaults(
  defineProps<{
    fluid?: boolean
    /** 切換平台 ghost 按鈕：目標 URL（未傳則不顯示） */
    switchUrl?: string
    /** 切換平台 ghost 按鈕：顯示文字（預設「切換平台」） */
    switchLabel?: string
  }>(),
  { fluid: false, switchLabel: '切換平台' }
)
</script>

<template>
  <div class="h-14 border-b border-stroke bg-bg-container">
    <div
      class="mx-auto flex h-full items-center gap-4 px-4 md:px-6"
      :class="fluid ? 'max-w-full' : 'max-w-7xl'"
    >
      <div class="flex h-full min-w-0 shrink-0 items-center">
        <slot name="brand" />
      </div>
      <nav class="flex h-full flex-1 items-center justify-center text-body2 text-fg-secondary-default">
        <slot />
      </nav>
      <div class="flex h-full min-w-0 shrink-0 items-center gap-3">
        <!-- countdown slot：排最左，切換鈕夾在 countdown 與 actions 之間 -->
        <slot name="countdown" />
        <a
          v-if="switchUrl"
          :href="switchUrl"
          target="_blank"
          rel="noopener noreferrer"
          class="border-stroke text-fg-secondary-default hover:bg-bg-container-subtle inline-flex h-9 items-center gap-1.5 rounded-md border bg-transparent px-3 text-sm font-medium whitespace-nowrap transition-colors"
        >
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" class="h-[15px] w-[15px] shrink-0" aria-hidden="true">
            <path d="m16 3 4 4-4 4"/><path d="M20 7H4"/><path d="m8 21-4-4 4-4"/><path d="M4 17h16"/>
          </svg>
          {{ switchLabel }}
        </a>
        <slot name="actions" />
      </div>
    </div>
  </div>
</template>
