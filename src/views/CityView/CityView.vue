<template>
  <div>
    <Suspense>
      <AsyncCityView/>

      <template #fallback>
        <div class="loading">
          loading....
        </div>
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityView from "@/components/AsyncCityView/AsyncCityView.vue";
</script>