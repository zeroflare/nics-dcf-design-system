<script setup lang="ts">
/**
 * Button — DS 統一按鈕元件（2026.9.12 拍板）
 * variant: default · secondary · outline · ghost · destructive · link（secondary / link 保留未使用）
 * size: xs(h-6) · sm(h-8) · default(h-9) · lg(h-10) · icon(36px) · icon-sm(32px) · icon-lg(40px)
 * 規則（2026.7.19）：icon 系列一律 size="icon-sm"；頁碼類用 size="icon"；不得以 class 覆寫尺寸
 * 規則（2026.7.29）：xs 用於 card/dialog 內行內小按鈕，字級對應 caption (12px)
 * 依賴契約：消費端需安裝 reka-ui、class-variance-authority，且 @ 別名指向 src
 */
import { cva, type VariantProps } from 'class-variance-authority'
import type { PrimitiveProps } from 'reka-ui'
import type { HTMLAttributes } from 'vue'
import { Primitive } from 'reka-ui'
import { cn } from '@/lib/utils'

const buttonVariants = cva(
  [
    'inline-flex items-center justify-center gap-2 whitespace-nowrap',
    'rounded-[var(--radius-md)] font-medium transition-all cursor-pointer',
    'disabled:pointer-events-none disabled:opacity-50',
    "[&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0",
    'outline-none focus-visible:ring-2 focus-visible:ring-offset-0',
    'focus-visible:ring-[var(--color-primary-solid)]/40',
  ].join(' '),
  {
    variants: {
      variant: {
        default:
          'bg-[var(--color-primary-solid)] text-[var(--color-fg-inverted-default)] hover:bg-[var(--color-primary-solid-hover)]',
        secondary:
          'bg-[var(--color-bg-interactive)] text-[var(--color-fg-primary-default)] hover:bg-[var(--color-bg-interactive-hover)]',
        outline:
          'border border-[var(--color-stroke)] bg-transparent text-[var(--color-fg-primary-default)] hover:bg-[var(--color-bg-interactive)]',
        ghost:
          'bg-transparent text-[var(--color-fg-secondary-default)] hover:bg-[var(--color-bg-interactive)] hover:text-[var(--color-fg-primary-default)]',
        destructive:
          'bg-[var(--color-danger-solid)] text-[var(--color-fg-inverted-default)] hover:opacity-90 focus-visible:ring-[var(--color-danger-solid)]/40',
        link: 'text-[var(--color-primary-fg)] underline-offset-4 hover:underline',
      },
      size: {
        xs: "h-6 gap-1 px-2 text-[length:var(--text-caption)] [&_svg:not([class*='size-'])]:size-3",
        sm: 'h-8 gap-1.5 px-3 text-[length:var(--text-button)] has-[>svg]:px-2.5',
        default: 'h-9 px-4 py-2 text-[length:var(--text-button)] has-[>svg]:px-3',
        lg: 'h-10 px-6 text-[length:var(--text-button)] has-[>svg]:px-4',
        icon: 'size-9',
        'icon-sm': 'size-8',
        'icon-lg': 'size-10',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  }
)

export type ButtonVariants = VariantProps<typeof buttonVariants>
export { buttonVariants }

interface Props extends PrimitiveProps {
  variant?: ButtonVariants['variant']
  size?: ButtonVariants['size']
  class?: HTMLAttributes['class']
}

const props = withDefaults(defineProps<Props>(), {
  as: 'button',
})
</script>

<template>
  <Primitive
    data-slot="button"
    :data-variant="variant"
    :data-size="size"
    :as="as"
    :as-child="asChild"
    :class="cn(buttonVariants({ variant, size }), props.class)"
  >
    <slot />
  </Primitive>
</template>
