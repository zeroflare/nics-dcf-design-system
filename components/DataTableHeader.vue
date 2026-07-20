<script setup lang="ts">
/** DataTable 表頭容器:淺灰底 + stroke 分隔線,詳見 DataTable.vue */
import type { HTMLAttributes } from 'vue'
import { TableHeader } from '@/components/ui/table'
import { cn } from '@/lib/utils'

const props = defineProps<{ class?: HTMLAttributes['class'] }>()
</script>

<template>
  <TableHeader :class="cn('bg-bg-container-subtle [&_tr]:border-stroke [&_tr]:hover:bg-transparent', props.class)">
    <slot />
  </TableHeader>
</template>
