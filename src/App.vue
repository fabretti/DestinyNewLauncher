<script setup lang="ts">
import { computed } from 'vue'
import { RouterView, useRoute } from 'vue-router'
import HeaderBlock from './components/HeaderBlock.vue'

const currentYear = new Date().getFullYear()
const route = useRoute()

const isAuthPage = computed(() => route.name === 'auth')
</script>

<template>
  <HeaderBlock v-if="!isAuthPage" />
  <RouterView />
  <footer>
    <div class="text-body-14 color-grey">Destiny Aion {{ currentYear }}</div>
  </footer>
  <img src="@/assets/img/MainBg.png" alt="bg" class="bg-image" />
</template>

<style lang="scss">
body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.bg-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  object-fit: cover;
  backdrop-filter: blur(8px);
}
</style>
