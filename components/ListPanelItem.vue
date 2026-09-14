<script setup lang="ts">
/**
 * ListPanelItem — 左側列表的單行項目
 *
 * variant="card"    platform 風格：左側 4px 色條（border-l-4）佔位對齊
 * variant="sidebar" access 風格：選中時顯示絕對定位色條（不佔版面）
 *
 * 呼叫端透過 default slot 填入名稱、Badge、副文字等內容
 */
export interface ListPanelItemProps {
  selected?: boolean
  variant?: 'card' | 'sidebar'
}

const props = withDefaults(defineProps<ListPanelItemProps>(), {
  selected: false,
  variant: 'card',
})

const emit = defineEmits<{
  click: []
}>()
</script>

<template>
  <div
    class="border-stroke relative cursor-pointer border-b px-4 py-3 transition-colors"
    :class="[
      props.variant === 'card'
        ? props.selected
          ? 'bg-selected-bg border-l-4 border-l-primary-solid'
          : 'bg-bg-container hover:bg-bg-container-subtle border-l-4 border-l-transparent'
        : props.selected
          ? 'bg-bg-container'
          : 'bg-bg-container hover:bg-bg-container-subtle',
    ]"
    @click="emit('click')"
  >
    <!-- sidebar 模式：絕對定位色條，不佔版面寬度 -->
    <span
      v-if="props.variant === 'sidebar' && props.selected"
      aria-hidden="true"
      class="pointer-events-none absolute inset-y-0 left-0.5 w-1 bg-primary-solid"
    />
    <slot />
  </div>
</template>
