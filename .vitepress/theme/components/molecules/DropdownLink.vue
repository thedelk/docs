<script setup lang="ts">
import { ref, watch } from 'vue'
import { useRoute } from 'vitepress'

import type { DefaultTheme } from '@/config'
import type { PropType } from 'vue'

defineProps({
  item: {
    type: Object as PropType<DefaultTheme.NavItemWithChildren>,
    required: true,
  },
})

const route = useRoute()
const open = ref(false)
watch(
  () => route.path,
  () => {
    open.value = false
  },
)
function toggle() {
  open.value = !open.value
}
</script>

<template>
  <div class="relative inline-flex items-center" @mouseenter="open = true" @mouseleave="open = false">
    <button class="p-2 inline-flex items-center" :aria-label="item.ariaLabel" @click="toggle">
      {{ item.text }}
      <uil:angle-down class="ml-2 transition-transform duration-50 transform" :class="{'rotate-180': open}" />
    </button>

    <ul
      v-if="open"
      class="
        absolute top-10 right-0 z-50
        p-2 min-w-40
        rounded-md border
        bg-$windi-bg
        border-gray-100 dark:border-dark-400
      "
    >
      <li v-for="nav in item.items" :key="nav.text">
        <div v-if="nav.text === 'separator'" class="border-b border-$windi-bc my-1 mx-3" />
        <NavLink v-else :item="nav" :dropdown="true" />
      </li>
    </ul>
  </div>
</template>
