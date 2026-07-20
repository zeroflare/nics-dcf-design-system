<script setup lang="ts">
/**
 * Tabs — 底線式頁籤 (2026.7.20 盤點拍板為正式規格,取代已刪除的 shadcn 膠囊式)
 * 實體住在 nics-dcf-design-system,消費端由 @nics/design-tokens/components/Tabs.vue 引用
 * 規格:px-6 py-3、文字 Body2 (14px);選中態 primary-solid 底線 2px + 700 字重 + primary-fg 文字;
 * 未選 fg-secondary-default,hover 走同階的 fg-secondary-hover ( 2026.7.20 狀態 token 上線後改用 )
 * disabled tab ( 2026.7.20 資料來源頁需求新增 ):fg-secondary-disabled + cursor-not-allowed,
 * hint 以 shadcn Tooltip 呈現停用原因 ( 設計師反饋原生 title 太陽春,改回 Tooltip )
 * 依賴契約:消費端須已依接入指南安裝 shadcn 的 tooltip,且 @ 別名指向 src
 */
import { Tooltip, TooltipContent, TooltipProvider, TooltipTrigger } from '@/components/ui/tooltip'

export interface TabItem {
  value: string
  label: string
  disabled?: boolean
  /** disabled 時的提示文字,以 shadcn Tooltip 呈現 */
  hint?: string
}

const props = defineProps<{
  tabs: TabItem[]
  modelValue: string
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>()

function select(tab: TabItem) {
  if (tab.disabled) return
  if (tab.value !== props.modelValue) emit('update:modelValue', tab.value)
}

function tabClass(tab: TabItem): string {
  if (tab.disabled) return 'cursor-not-allowed font-normal text-fg-secondary-disabled'
  if (tab.value === props.modelValue)
    return 'border-b-2 border-primary-solid font-bold text-primary-fg'
  return 'font-normal text-fg-secondary-default hover:text-fg-secondary-hover'
}
</script>

<template>
  <div class="flex border-b border-stroke" role="tablist">
    <template v-for="tab in tabs" :key="tab.value">
      <TooltipProvider v-if="tab.disabled && tab.hint">
        <Tooltip>
          <TooltipTrigger as-child>
            <button
              type="button"
              role="tab"
              aria-disabled="true"
              class="px-6 py-3 text-body2 transition-colors"
              :class="tabClass(tab)"
            >
              {{ tab.label }}
            </button>
          </TooltipTrigger>
          <TooltipContent>{{ tab.hint }}</TooltipContent>
        </Tooltip>
      </TooltipProvider>
      <button
        v-else
        type="button"
        role="tab"
        :aria-selected="tab.value === modelValue"
        :aria-disabled="tab.disabled || undefined"
        class="px-6 py-3 text-body2 transition-colors"
        :class="tabClass(tab)"
        @click="select(tab)"
      >
        {{ tab.label }}
      </button>
    </template>
  </div>
</template>
