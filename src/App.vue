<script setup>
import { RouterView } from 'vue-router'
import SiteNavigation from "@/components/SiteNavigation/SiteNavigation.vue";
</script>

<template>
  <div class="flex flex-col min-h-screen bg-weather-primary font-Roboto">
    <site-navigation />
    <RouterView />
  </div>
</template>

<style scoped>

</style>
