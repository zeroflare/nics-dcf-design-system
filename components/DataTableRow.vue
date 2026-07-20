<script setup lang="ts">
/** DataTable 資料列:stroke 分隔線 + subtle hover;不需要 hover 的列 ( 表頭列 / 空狀態列 ) 傳 class="hover:bg-transparent",詳見 DataTable.vue */
import type { HTMLAttributes } from 'vue'
import { TableRow } from '@/components/ui/table'
import { cn } from '@/lib/utils'

const props = defineProps<{ class?: HTMLAttributes['class'] }>()
</script>

<template>
  <TableRow :class="cn('border-stroke hover:bg-bg-container-subtle', props.class)">
    <slot />
  </TableRow>
</template>
