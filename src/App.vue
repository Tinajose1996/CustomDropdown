<script setup>
import { RouterLink, RouterView } from 'vue-router'
import ParentComponent from './components/ParentComponent.vue'
</script>

<template>
  <div>
    <div class="wrapper">
      <ParentComponent />
    </div>
  </div>

  <RouterView />
</template>

<style scoped>
</style>
