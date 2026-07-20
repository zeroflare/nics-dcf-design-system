<script setup lang="ts">
/** DataTable 表頭儲存格:Caption 粗體大寫 + fg-secondary,垂直內距依 density 兩檔制,詳見 DataTable.vue */
import { inject, computed } from 'vue'
import type { ComputedRef, HTMLAttributes } from 'vue'
import { TableHead } from '@/components/ui/table'
import { cn } from '@/lib/utils'

const props = defineProps<{ class?: HTMLAttributes['class'] }>()

const density = inject<ComputedRef<'default' | 'dense'>>(
  'dcf-datatable-density',
  computed(() => 'default' as const)
)
</script>

<template>
  <TableHead
    :class="
      cn(
        'h-auto px-6 text-caption font-bold tracking-wider text-fg-secondary uppercase',
        density === 'dense' ? 'py-3' : 'py-4',
        props.class
      )
    "
  >
    <slot />
  </TableHead>
</template>
