<template>
  <div class="text">
    <h1>🎉 Happy New Year</h1>

    <AnimatedYear />

    <p>May your year be filled with success and happiness ✨</p>
  </div>
</template>

<script setup>
  import AnimatedYear from '@/components/AnimatedYear.vue'
</script>

<style scoped>
  .text {
    position: relative;
    z-index: 2;
    text-align: center;
    color: white;
  }

  h1 {
    font-size: 3rem;
    margin-bottom: 1rem;
  }

  p {
    margin-top: 1rem;
    opacity: 0.85;
  }
</style>