<template>
  <GalaxyBackground v-if="phase !== 'idle'" />
  <Fireworks v-if="phase === 'finished'" />

  <main class="card">
    <NewYearText :phase="phase" @start="startSequence" />
  </main>

  <Footer :show="phase === 'finished'" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import GalaxyBackground from '@/components/GalaxyBackground.vue'
  import Fireworks from '@/components/Fireworks.vue'
  import NewYearText from '@/components/NewYearText.vue'
  import Footer from '@/components/Footer.vue'

  const phase = ref<'idle' | 'transition' | 'finished'>('idle')

  function startSequence() {
    phase.value = 'transition'

    setTimeout(() => {
      phase.value = 'finished'
    }, 1800)
  }
</script>adwda

<style scoped>
  .card {
    position: relative;
    z-index: 2;
    min-height: 100svh;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>