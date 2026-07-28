<script setup lang="ts">
/** DataTable 表頭儲存格:Caption 粗體大寫 + fg-secondary,垂直內距固定 12px ( 表頭恆為精簡高度,不隨 density 變動;body 儲存格才依 density,詳見 DataTable.vue。2026.7.28 拍板:原 default 16px 太高,一律降為 12px ) */
import type { HTMLAttributes } from 'vue'
import { TableHead } from '@/components/ui/table'
import { cn } from '@/lib/utils'

const props = defineProps<{ class?: HTMLAttributes['class'] }>()
</script>

<template>
  <TableHead
    :class="
      cn(
        'h-auto px-6 py-3 text-caption font-bold tracking-wider text-fg-secondary-default uppercase',
        props.class
      )
    "
  >
    <slot />
  </TableHead>
</template>
