<template>
  <GalaxyBackground />
  <Fireworks />

  <main class="card">
    <NewYearText />
  </main>
</template>

<script setup>
  import GalaxyBackground from '@/components/GalaxyBackground.vue'
  import Fireworks from '@/components/Fireworks.vue'
  import NewYearText from '@/components/NewYearText.vue'
</script>

<style scoped>
  .card {
    position: relative;
    z-index: 2;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
