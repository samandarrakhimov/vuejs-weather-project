<template>
  <div class="flex flex-col bg-blue-700 min-h-screen font-serif">
    <SiteNavigation/>
    <RouterView class="flex-1" v-slot="{ Component }">
      <Transition name="page">
        <component :is="Component" />
      </Transition>
    </RouterView>
  </div>
</template>
<script setup>
import {RouterView} from 'vue-router'
import SiteNavigation from './components/siteNavigation.vue';
</script>
<style>
.page-enter-active {
  transition: 600ms ease all;
}

.page-enter-from {
  opacity: 0;
}
</style>