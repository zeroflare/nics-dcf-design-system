<script setup lang="ts">
/**
 * Tag — Component 層 Tag token 的正式載體 (2026.7.20 盤點拍板新增)
 * 實體住在 nics-dcf-design-system,消費端由 @nics/design-tokens/components/Tag.vue 引用
 * 語意 5 種 ( 50 底 / 700 字 ) + 分類 12 種 ( 100 底 / 700 字 ),
 * 深淺差異本身就是「狀態 vs 分類」的規則。無框、膠囊形,文字 text-tag ( 12 / 500 / lh 1 )
 */
export type TagType =
  | 'success'
  | 'warning'
  | 'danger'
  | 'info'
  | 'neutral'
  | 'cat-authenticate'
  | 'cat-safety-lock'
  | 'cat-dataset'
  | 'cat-collaboration'
  | 'cat-record'
  | 'cat-account'
  | 'cat-role'
  | 'cat-system'
  | 'cat-resource-access'
  | 'cat-operation'
  | 'cat-dataset-access'
  | 'cat-inbox'

const props = defineProps<{ type: TagType }>()

// Tailwind JIT 需要完整字面值,不能動態組字串
const TYPE_CLASS: Record<TagType, string> = {
  'success': 'bg-tag-success-bg text-tag-success-fg',
  'warning': 'bg-tag-warning-bg text-tag-warning-fg',
  'danger': 'bg-tag-danger-bg text-tag-danger-fg',
  'info': 'bg-tag-info-bg text-tag-info-fg',
  'neutral': 'bg-tag-neutral-bg text-tag-neutral-fg',
  'cat-authenticate': 'bg-tag-cat-authenticate-bg text-tag-cat-authenticate-fg',
  'cat-safety-lock': 'bg-tag-cat-safety-lock-bg text-tag-cat-safety-lock-fg',
  'cat-dataset': 'bg-tag-cat-dataset-bg text-tag-cat-dataset-fg',
  'cat-collaboration': 'bg-tag-cat-collaboration-bg text-tag-cat-collaboration-fg',
  'cat-record': 'bg-tag-cat-record-bg text-tag-cat-record-fg',
  'cat-account': 'bg-tag-cat-account-bg text-tag-cat-account-fg',
  'cat-role': 'bg-tag-cat-role-bg text-tag-cat-role-fg',
  'cat-system': 'bg-tag-cat-system-bg text-tag-cat-system-fg',
  'cat-resource-access': 'bg-tag-cat-resource-access-bg text-tag-cat-resource-access-fg',
  'cat-operation': 'bg-tag-cat-operation-bg text-tag-cat-operation-fg',
  'cat-dataset-access': 'bg-tag-cat-dataset-access-bg text-tag-cat-dataset-access-fg',
  'cat-inbox': 'bg-tag-cat-inbox-bg text-tag-cat-inbox-fg',
}
</script>

<template>
  <span
    class="inline-flex items-center rounded-full px-3 py-1 text-tag whitespace-nowrap"
    :class="TYPE_CLASS[props.type]"
  >
    <slot />
  </span>
</template>
