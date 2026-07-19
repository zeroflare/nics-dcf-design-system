<!-- 確認型對話框的統一樣式：置中標題與說明、無圖示、無右上角關閉鈕、左取消右主動作 -->
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
    <DialogContent :show-close-button="false" class="sm:max-w-sm">
      <DialogHeader class="items-center text-center sm:text-center">
        <DialogTitle>{{ title }}</DialogTitle>
        <DialogDescription v-if="description" class="leading-relaxed">
          {{ description }}
        </DialogDescription>
      </DialogHeader>
      <slot />
      <DialogFooter>
        <Button variant="outline" @click="openModel = false">取消</Button>
        <Button :variant="variant ?? 'default'" :disabled="confirmDisabled" @click="emit('confirm')">
          {{ confirmLabel }}
        </Button>
      </DialogFooter>
    </DialogContent>
  </Dialog>
</template>
