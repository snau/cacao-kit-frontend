<script setup lang="ts">
// This Nuxt page will render the about page

import type { KirbyAboutResponse } from '~/queries'
import { aboutQuery } from '~/queries'

defineI18nRoute({
  paths: {
    en: '/about',
    de: '/ueber-uns',
  },
})

const { locale, t } = useI18n()
const content = ref<HTMLElement | undefined>()
useInternalLinks(content)

const { data, error } = await useKql<KirbyAboutResponse>(aboutQuery, {
  language: locale.value,
})

// Store page data
const page = data.value?.result
if (page) {
  setPage(page)
}
</script>

<template>
  <div>
    <DevOnly>
      <AppDebugHelper :error="error" />
    </DevOnly>

    <div v-if="error || !page" class="error-state">
      <h1>{{ t('errors.pageNotFound') || 'Page not found' }}</h1>
      <p>{{ error?.message || 'Unable to load page content' }}</p>
    </div>

    <template v-else>
      <KirbyLayouts
        v-if="page?.layouts?.length"
        :layouts="page.layouts as any"
      />

      <br />

      <header>
        <h2>{{ t('about.getInContact') }}</h2>
        <div ref="content" class="grid" style="--gutter: 1.5rem">
          <section class="column text" style="--columns: 4">
            <h3>{{ t('about.address') }}</h3>
            <div v-html="page?.address" />
          </section>

          <section class="column text" style="--columns: 4">
            <h3>{{ t('about.email') }}</h3>
            <p v-html="page?.email" />
            <h3>{{ t('about.phone') }}</h3>
            <p v-html="page?.phone" />
          </section>

          <section class="column text" style="--columns: 4">
            <h3>{{ t('about.onTheWeb') }}</h3>
            <ul>
              <li v-for="(item, index) in page?.social" :key="index">
                <a :href="item.url">
                  {{ item.platform }}
                </a>
              </li>
            </ul>
          </section>
        </div>
      </header>
    </template>
  </div>
</template>
