<template>
  <span>{{ display }}</span>
</template>

<script setup lang="ts">
  import { ref, onMounted } from 'vue'

  const display = ref('2025')
  const target = '2026'

  onMounted(() => {
    const duration = 1400
    const start = performance.now()

    function tick(now: number) {
      const p = Math.min((now - start) / duration, 1)

      let out = ''
      for (let i = 0; i < target.length; i++) {
        out += p > i / target.length
          ? target[i]
          : Math.floor(Math.random() * 10)
      }

      display.value = out

      if (p < 1) requestAnimationFrame(tick)
      else display.value = target
    }

    requestAnimationFrame(tick)
  })
</script>
