<script setup lang="ts">
/**
 * Tabs — 底線式頁籤 (2026.7.20 盤點拍板為正式規格,取代已刪除的 shadcn 膠囊式)
 * 實體住在 nics-dcf-design-system,消費端由 @nics/design-tokens/components/Tabs.vue 引用
 * 規格:px-6 py-3、文字 Body2 (14px);選中態 primary-solid 底線 2px + 700 字重 + primary-fg 文字;
 * 未選 fg-secondary (400),hover 加深為 fg-primary
 */
export interface TabItem {
  value: string
  label: string
}

const props = defineProps<{
  tabs: TabItem[]
  modelValue: string
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>()

function select(value: string) {
  if (value !== props.modelValue) emit('update:modelValue', value)
}
</script>

<template>
  <div class="flex border-b border-stroke" role="tablist">
    <button
      v-for="tab in tabs"
      :key="tab.value"
      type="button"
      role="tab"
      :aria-selected="tab.value === modelValue"
      class="px-6 py-3 text-body2 transition-colors"
      :class="
        tab.value === modelValue
          ? 'border-b-2 border-primary-solid font-bold text-primary-fg'
          : 'font-normal text-fg-secondary hover:text-fg-primary'
      "
      @click="select(tab.value)"
    >
      {{ tab.label }}
    </button>
  </div>
</template>
