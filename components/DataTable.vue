<script setup lang="ts">
/**
 * DataTable — 資料表格的統一封裝 (2026.7.20 盤點拍板新增)
 * 實體住 nics-dcf-design-system,消費端由 @nics/design-tokens/components/DataTable.vue 引用
 * 內部包消費端的 shadcn Table ( vendor ),烤入拍板樣式;搭配 DataTableHeader / Body / Row / Head / Cell 使用
 * density:default = 一般資料表格 ( 儲存格垂直 16px )、dense = 密集小表格 ( 12px ),間距章兩檔制
 * 依賴契約:消費端須已安裝 shadcn 的 table,且 @ 別名指向 src
 */
import { provide, computed } from 'vue'
import type { HTMLAttributes } from 'vue'
import { Table } from '@/components/ui/table'
import { cn } from '@/lib/utils'

const props = defineProps<{
  density?: 'default' | 'dense'
  class?: HTMLAttributes['class']
}>()

provide('dcf-datatable-density', computed(() => props.density ?? 'default'))
</script>

<template>
  <Table :class="cn('text-left text-body2', props.class)">
    <slot />
  </Table>
</template>
