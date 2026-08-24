<script setup lang="ts">
import { computed } from 'vue'

/**
 * Footer — 頁尾殼層 ( 2026.7.27 拍板新增，2026.7.30 比照存取節點 AppFooter.vue 定案內容全面改版，跨站共用 )
 * 實體住在 nics-dcf-design-system，消費端由 @nics/design-tokens/components/Footer.vue 引用。
 * 機關資訊 ( 數位發展部聯絡方式 )、版權列、整體結構固定，各站差異收斂為以下 props：
 *
 * siteLabel：logo 主名下方的小字，標示該站產品名稱 ( 存取節點：資料存取節點；分析平臺：管理平台 )；
 *            web=true 時忽略此 prop ( 主名單行顯示，無副名 )
 * homeHref：logo 連結目標，預設首頁 "/"
 * web：Web 端變體，預設 false。true 時主名字級放大至 18px ( body1+2px ) 且不顯示副名，
 *       對應 dcf-website-demo 公開頁面 footer 的設計規格 ( 2026.7.30 拍板 )
 */
withDefaults(
  defineProps<{
    siteLabel?: string
    homeHref?: string
    logoSrc?: string
    web?: boolean
  }>(),
  { homeHref: '/', logoSrc: '/favicon.png', web: false }
)

const year = computed(() => new Date().getFullYear())

const modaUrl = 'https://moda.gov.tw/'
const modaAddressUrl =
  'https://www.google.com.tw/maps/place/100臺北市中正區南門里延平南路143號/@25.0362981,121.508228,17z/data=!3m1!4b1!4m6!3m5!1s0x3442a9a6d27e2995:0xe6f09916f3255a16!8m2!3d25.0362981!4d121.508228!16s%2Fg%2F12jm1019x?entry=ttu&g_ep=EgoyMDI2MDgxOS4wIKXMDSoASAFQAw%3D%3D'
</script>

<template>
  <footer class="shrink-0 border-t border-stroke bg-neutral-200">
    <div class="mx-auto max-w-7xl px-4 py-8 md:px-6 lg:px-8">
      <div class="flex flex-col gap-8 lg:flex-row lg:items-start lg:justify-between">
        <!-- 品牌 -->
        <div class="flex items-center gap-2">
          <a :href="homeHref" class="shrink-0" title="政府資料匯流分析協作平臺首頁">
            <img
              :src="logoSrc"
              alt=""
              class="h-10 w-10 object-contain"
              width="40"
              height="40"
            />
          </a>
          <div>
            <p
              class="font-bold text-fg-primary-default"
              :style="web ? 'font-size:18px;line-height:1.5' : 'font-size:16px;line-height:1.5'"
            >政府資料匯流分析協作平臺</p>
            <p v-if="!web && siteLabel" class="text-caption text-fg-secondary-default">{{ siteLabel }}</p>
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
