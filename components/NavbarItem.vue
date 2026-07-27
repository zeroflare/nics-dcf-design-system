<script setup lang="ts">
/**
 * NavbarItem — Navbar 選單項 ( 2026.7.27 拍板新增，跨站共用 )
 * 實體住在 nics-dcf-design-system，消費端由 @nics/design-tokens/components/NavbarItem.vue 引用
 * 依賴契約：消費端須已安裝 vue-router ( 所有消費站都是 vue-router SPA，視為既有前提 )
 *
 * 逐字對照存取節點 AppNav.vue 選單項搬移，未選 fg-secondary、hover 加深、
 * 選中態 primary-fg 文字 + 700 字重 + primary-solid 底線 3px。
 * active 由消費端依自己的路由比對邏輯傳入 ( 不綁定特定路由命名慣例 )。
 * badge 為數字或文字徽章 ( 如未讀數量 )，未帶或為 0 時不顯示。
 */
import { RouterLink, type RouteLocationRaw } from 'vue-router'
import { computed } from 'vue'

const props = defineProps<{
  to: RouteLocationRaw
  active?: boolean
  badge?: string | number | null
}>()

const showBadge = computed(() => props.badge !== undefined && props.badge !== null && props.badge !== 0 && props.badge !== '')
</script>

<template>
  <RouterLink
    :to="to"
    class="relative flex h-full items-center px-4 whitespace-nowrap transition-colors hover:text-fg-secondary-hover"
    :class="active ? 'font-bold text-primary-fg after:absolute after:bottom-0 after:left-0 after:h-[3px] after:w-full after:bg-primary-solid' : ''"
  >
    <span class="relative">
      <slot />
      <span
        v-if="showBadge"
        class="pointer-events-none absolute top-0 left-full ml-0.5 inline-flex h-4 min-w-4 -translate-y-1/2 items-center justify-center rounded-full bg-danger-solid px-1 text-[0.6rem] font-bold text-white"
      >
        {{ badge }}
      </span>
    </span>
  </RouterLink>
</template>
