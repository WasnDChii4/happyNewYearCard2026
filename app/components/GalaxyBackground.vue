<template>
  <canvas ref="canvas"></canvas>
</template>

<script setup lang="ts">
  import { onMounted, ref } from 'vue'

  const canvas = ref<HTMLCanvasElement | null>(null)

  onMounted(() => {
    const ctx = canvas.value!.getContext('2d')!
    canvas.value!.width = window.innerWidth
    canvas.value!.height = window.innerHeight

    const stars = Array.from({ length: 150 }, () => ({
      x: Math.random() * canvas.value!.width,
      y: Math.random() * canvas.value!.height,
      r: Math.random() * 2,
      v: Math.random() * 0.5
    }))

    function animate() {
      ctx.clearRect(0, 0, canvas.value!.width, canvas.value!.height)
      stars.forEach(s => {
        ctx.beginPath()
        ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2)
        ctx.fillStyle = 'white'
        ctx.fill()
        s.y += s.v
        if (s.y > canvas.value!.height) s.y = 0
      })
      requestAnimationFrame(animate)
    }

    animate()
  })
</script>

<style scoped>
  canvas {
    position: fixed;
    inset: 0;
    z-index: 0;
  }
</style>
