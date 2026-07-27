<script setup lang="ts">
/**
 * Avatar — Component 層使用者頭像 ( 首字 + 主色漸層,2026.7.27 拍板新增 )
 * 實體住 nics-dcf-design-system,消費端由 @nics/design-tokens/components/Avatar.vue 引用。
 * 後台無上傳大頭貼功能,頭像一律取「名稱」首字 + brand 主色對角漸層底
 * ( token 見 tokens.css 的 --color-avatar-gradient-* )、白字置中。
 * size 同時決定圓徑與字級,尺寸與字級綁定確保比例協調;navbar 用 sm ( 24px )。
 */
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    name?: string | null
    size?: 'sm' | 'md' | 'lg'
  }>(),
  { name: '', size: 'md' }
)

// 取名稱第一個字元 ( 中文一個字 / 英文一個字母大寫 ),空值退回問號
const initial = computed(() => {
  const n = (props.name ?? '').trim()
  return n ? n.charAt(0).toUpperCase() : '?'
})

// CJK ( 中日韓漢字 ) 字身框比拉丁字母滿,同字級塞進小圓會顯得緊繃,偵測到就把字級降一階,讓圓內留白一致
const isCjk = computed(() => /[㐀-鿿豈-﫿]/.test(initial.value))

const SIZE: Record<'sm' | 'md' | 'lg', { box: string; latin: string; cjk: string }> = {
  sm: { box: 'h-6 w-6', latin: 'text-body2', cjk: 'text-caption' },
  md: { box: 'h-8 w-8', latin: 'text-body1', cjk: 'text-body2' },
  lg: { box: 'h-10 w-10', latin: 'text-subtitle', cjk: 'text-body1' },
}

const boxClass = computed(() => SIZE[props.size].box)
const textClass = computed(() => (isCjk.value ? SIZE[props.size].cjk : SIZE[props.size].latin))
</script>

<template>
  <span
    class="inline-flex shrink-0 items-center justify-center rounded-full bg-linear-to-br from-avatar-gradient-from to-avatar-gradient-to font-bold whitespace-nowrap text-fg-inverted-default select-none"
    :class="[boxClass, textClass]"
  >
    {{ initial }}
  </span>
</template>
