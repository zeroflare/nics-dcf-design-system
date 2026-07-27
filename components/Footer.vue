<script setup lang="ts">
/**
 * Footer — 頁尾殼層 ( 2026.7.27 拍板新增，跨站共用 )
 * 實體住在 nics-dcf-design-system，消費端由 @nics/design-tokens/components/Footer.vue 引用
 * 逐字對照存取節點 AppFooter.vue 搬移，結構固定，版權文字與連結由各站帶入。
 *
 * copyrightText：完整版權文字 ( 如「© 2026 政府資料匯流分析平臺｜資料存取節點」)，各站自行組字串傳入
 * links：右側連結，預設服務條款 / 隱私權政策兩項，各站可覆寫
 */
withDefaults(
  defineProps<{
    copyrightText: string
    links?: { label: string; href: string }[]
  }>(),
  {
    links: () => [
      { label: '服務條款', href: '#' },
      { label: '隱私權政策', href: '#' },
    ],
  }
)
</script>

<template>
  <footer class="shrink-0 border-t border-stroke bg-bg-container py-4">
    <div
      class="mx-auto flex max-w-[1024px] items-center justify-between px-6 text-caption tracking-widest text-fg-tertiary-default uppercase"
    >
      <span>{{ copyrightText }}</span>
      <div class="flex gap-6">
        <a
          v-for="link in links"
          :key="link.label"
          class="transition-colors hover:text-primary-fg"
          :href="link.href"
        >
          {{ link.label }}
        </a>
      </div>
    </div>
  </footer>
</template>
