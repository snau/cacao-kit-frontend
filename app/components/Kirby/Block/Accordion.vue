<script setup lang="ts">
import type { KirbyBlock } from '#nuxt-kql'

const { block } = defineProps<{
  block: KirbyBlock<'accordion'>
}>()
</script>

<template>
  <div class="w-full max-w-3xl mx-auto">
    <UAccordion
      multiple
      :ui="{
        item: '-mx-4 my-2 border-none rounded-xl text-[var(--ui-accordion-color-text)] bg-[var(--ui-accordion-color-closed)] hover:bg-[var(--ui-accordion-color-hover)] data-[state=open]:bg-[var(--ui-accordion-color-open)]',
        body: 'text-base',
        trigger: 'text-base font-bold  p-6 cursor-pointer',
        content: 'px-12 ',
      }"
      :items="
        (
          (block.content.accordion || []) as Array<{
            label: string
            icon?: string
            content: string
            disabled?: boolean
          }>
        ).map((item) => ({
          label: item.label,
          icon: item.icon,
          content: item.content,
          disabled: item.disabled || false,
        }))
      "
    />
  </div>
</template>
