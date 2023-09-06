<template>
  <div>
    <Suspense>
      <template  #default>
        <AsyncCityView />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityView from "./AsyncCityView.vue";
</script>