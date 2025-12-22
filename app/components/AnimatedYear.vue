<template>
  <h2 class="year">{{ displayYear }}</h2>
</template>

<script setup lang="ts">
  import { ref, onMounted } from 'vue'

  const startYear = '2025'
  const endYear = '2026'

  const displayYear = ref(startYear)

  function animateYear() {
    const duration = 1500 // ms
    const startTime = performance.now()

    function update(now: number) {
      const progress = Math.min((now - startTime) / duration, 1)

      let result = ''
      for (let i = 0; i < endYear.length; i++) {
        if (progress > i / endYear.length) {
          result += endYear[i]
        } else {
          result += Math.floor(Math.random() * 10)
        }
      }

      displayYear.value = result

      if (progress < 1) {
        requestAnimationFrame(update)
      } else {
        displayYear.value = endYear
      }
    }

    requestAnimationFrame(update)
  }

  onMounted(() => {
    setTimeout(animateYear, 800)
  })
</script>

<style scoped>
  .year {
    font-size: 6rem;
    font-weight: 800;
    color: #ffd700;
    letter-spacing: 0.15em;
    text-shadow:
      0 0 10px rgba(255, 215, 0, 0.6),
      0 0 25px rgba(255, 215, 0, 0.4);
  }
</style>
