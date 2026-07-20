<script setup lang="ts">
/** DataTable 儲存格:垂直內距依 density 兩檔制 ( 16px / 12px ),文字色由呼叫端指定 ( fg-primary / fg-secondary 等 ),詳見 DataTable.vue */
import { inject, computed } from 'vue'
import type { ComputedRef, HTMLAttributes } from 'vue'
import { TableCell } from '@/components/ui/table'
import { cn } from '@/lib/utils'

const props = defineProps<{ class?: HTMLAttributes['class'] }>()

const density = inject<ComputedRef<'default' | 'dense'>>(
  'dcf-datatable-density',
  computed(() => 'default' as const)
)
</script>

<template>
  <TableCell :class="cn('px-6', density === 'dense' ? 'py-3' : 'py-4', props.class)">
    <slot />
  </TableCell>
</template>
