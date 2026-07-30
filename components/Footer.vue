<script setup lang="ts">
import { computed } from 'vue'

/**
 * Footer — 頁尾殼層 ( 2026.7.27 拍板新增，2026.7.30 比照存取節點 AppFooter.vue 定案內容全面改版，跨站共用 )
 * 實體住在 nics-dcf-design-system，消費端由 @nics/design-tokens/components/Footer.vue 引用。
 * 機關資訊 ( 數位發展部聯絡方式 )、版權列、整體結構固定，兩站唯一的差異是 logo 主名下方的副名。
 *
 * siteLabel：logo 主名下方的小字，標示該站產品名稱 ( 存取節點：資料存取節點；分析平臺：管理平台 )
 * homeHref：logo 連結目標，預設首頁 "/"
 */
withDefaults(
  defineProps<{
    siteLabel: string
    homeHref?: string
  }>(),
  { homeHref: '/' }
)

const year = computed(() => new Date().getFullYear())

const modaUrl = 'https://moda.gov.tw/'
const modaAddressUrl =
  'https://www.google.com/maps/place/100%E5%B9%B4%E5%8F%B0%E5%8C%97%E5%B8%82%E4%B8%AD%E6%AD%A3%E5%8D%80%E5%BB%B6%E5%B9%B3%E5%8D%97143%E8%99%9F'
</script>

<template>
  <footer class="shrink-0 border-t border-stroke bg-neutral-200">
    <div class="mx-auto max-w-7xl px-4 py-8 md:px-6 lg:px-8">
      <div class="flex flex-col gap-8 lg:flex-row lg:items-start lg:justify-between">
        <!-- 品牌 -->
        <div class="flex items-center gap-2">
          <a :href="homeHref" class="shrink-0" title="政府資料匯流分析協作平臺首頁">
            <img
              src="/favicon.png"
              alt=""
              class="h-10 w-10 object-contain"
              width="40"
              height="40"
            />
          </a>
          <div>
            <p class="text-body1 font-bold text-fg-primary-default">政府資料匯流分析協作平臺</p>
            <p class="text-caption text-fg-secondary-default">{{ siteLabel }}</p>
          </div>
        </div>

        <!-- 機關資訊 -->
        <div class="lg:ml-auto lg:text-left">
          <a
            :href="modaUrl"
            target="_blank"
            rel="noopener noreferrer"
            class="text-body2 font-bold text-fg-primary-default transition-colors hover:text-fg-primary-hover"
          >
            數位發展部 Ministry of Digital Affairs
          </a>
          <dl class="mt-2 flex flex-col gap-1 text-body2 text-fg-secondary-default">
            <div class="flex gap-1">
              <dt class="shrink-0 text-fg-secondary-default">電話：</dt>
              <dd>(02) 2531-1998</dd>
            </div>
            <div class="flex gap-1">
              <dt class="shrink-0 text-fg-secondary-default">地址：</dt>
              <dd>
                <a
                  :href="modaAddressUrl"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="transition-colors hover:text-fg-primary-hover"
                >
                  100057 臺北市中正區延平南路143號
                </a>
              </dd>
            </div>
          </dl>
        </div>
      </div>
    </div>

    <!-- 版權列 -->
    <div class="border-t border-neutral-300 bg-neutral-200">
      <div
        class="mx-auto max-w-7xl px-4 py-4 text-center text-caption text-fg-secondary-default md:px-6 lg:px-8"
      >
        © {{ year }} 數位發展部
      </div>
    </div>
  </footer>
</template>
