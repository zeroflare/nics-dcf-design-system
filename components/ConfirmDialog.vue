<!-- 確認型對話框的統一樣式：靠左標題與說明 ( 2026.7.20 由置中改為靠左 )、無圖示、無右上角關閉鈕、左取消右主動作 -->
<!-- 高度上限 max-h-[80vh] + overflow-y-auto ( 2026.8.27 拍板 )：內容過長 ( 如帶長文字輸入框的 slot ) 時
     整個對話框改為內部捲動，不隨內容無限撐高超出視窗；slot 內若有 maxlength 限制的多行輸入框，
     建議該輸入框自己也設 max-h-* 讓長文字優先在欄位內部捲動，維持標題/按鈕列在畫面上可見 -->
<!-- 實體住在 nics-dcf-design-system,消費端由 @nics/design-tokens/components/ConfirmDialog.vue 引用 -->
<!-- 依賴契約:消費端須已依接入指南安裝 shadcn 的 dialog 與 button,且 @ 別名指向 src -->
<script setup lang="ts">
import { computed } from 'vue'
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog'
import { Button } from '@/components/ui/button'

const props = defineProps<{
  open: boolean
  title: string
  description?: string
  confirmLabel: string
  /** 主動作按鈕的 variant，破壞性操作用 destructive */
  variant?: 'default' | 'destructive'
  confirmDisabled?: boolean
  /** 取消鈕文字,預設「取消」( 2026.7.20 資料來源頁「繼續編輯」需求新增 ) */
  cancelLabel?: string
  /** 隱藏取消鈕，僅顯示主動作按鈕，用於單一出口的通知型對話框，預設 true */
  showCancel?: boolean
  /** 鎖定對話框，禁止點擊遮罩或按 Escape 關閉，用於工作階段逾時等強制操作場景 */
  persistent?: boolean
}>()

const emit = defineEmits<{
  (e: 'confirm'): void
  (e: 'close'): void
}>()

const openModel = computed({
  get: () => props.open,
  set: (value: boolean) => {
    if (!value) emit('close')
  },
})
</script>

<template>
  <Dialog v-model:open="openModel">
    <DialogContent
      :show-close-button="false"
      class="sm:max-w-sm max-h-[80vh] overflow-y-auto"
      v-bind="persistent ? { onInteractOutside: (e: Event) => e.preventDefault(), onEscapeKeyDown: (e: Event) => e.preventDefault() } : {}"
    >
      <DialogHeader class="text-left">
        <DialogTitle>{{ title }}</DialogTitle>
        <DialogDescription v-if="description" class="leading-relaxed">
          {{ description }}
        </DialogDescription>
      </DialogHeader>
      <slot />
      <DialogFooter>
        <Button v-if="showCancel !== false" variant="outline" @click="openModel = false">{{ cancelLabel ?? '取消' }}</Button>
        <Button :variant="variant ?? 'default'" :disabled="confirmDisabled" @click="emit('confirm')">
          {{ confirmLabel }}
        </Button>
      </DialogFooter>
    </DialogContent>
  </Dialog>
</template>
