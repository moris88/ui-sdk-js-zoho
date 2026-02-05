<script setup lang="ts">
import { useNav } from '@slidev/client'

const { currentPage, total } = useNav()
</script>

<template>
  <div
    class="slidev-layout default relative"
    :class="{ 'flex flex-col items-center justify-center h-full': currentPage === 1 }"
  >
    <slot />

    <!-- Footer solo se non cover -->
    <div
      v-if="currentPage !== 1"
      class="absolute bottom-4 right-6 text-sm opacity-70"
    >
      {{ currentPage }} / {{ total }}
    </div>
  </div>
</template>