<script setup lang="ts">
/**
 * Shimmer — Component 層載入骨架 ( 掃光效果,2026.7.27 拍板新增 )
 * 實體住 nics-dcf-design-system,消費端由 @nics/design-tokens/components/Shimmer.vue 引用。
 * 一道亮帶由左掃到右 ( 效果與 token 見 tokens.css 的 .shimmer 與 --color-skeleton-* ),
 * 取代 shadcn skeleton 的 animate-pulse。骨架塊尺寸與圓角由呼叫端 class 決定 ( 佈局覆寫 ),
 * 預設圓角 md;要做成圓點 / 膠囊 ( 如頭像、標籤佔位 ) 由呼叫端覆寫 rounded-full。
 */
import type { HTMLAttributes } from 'vue'
import { cn } from '@/lib/utils'

const props = defineProps<{ class?: HTMLAttributes['class'] }>()
</script>

<template>
  <div :class="cn('shimmer rounded-md', props.class)" />
</template>
