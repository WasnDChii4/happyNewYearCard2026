<template>
  <canvas ref="canvas"></canvas>
</template>

<script setup lang="ts">
  import { onMounted, ref } from 'vue'

  const canvas = ref<HTMLCanvasElement | null>(null)

  onMounted(() => {
    const c = canvas.value!
    const ctx = c.getContext('2d')!

    function resize() {
      c.width = window.innerWidth
      c.height = window.innerHeight
    }
    resize()
    window.addEventListener('resize', resize)

    const stars = Array.from({ length: 240 }, () => ({
      x: Math.random() * c.width,
      y: Math.random() * c.height,
      r: Math.random() * 1.2 + 0.4,
      v: Math.random() * 0.3 + 0.08 
    }))

    function animate() {
      ctx.fillStyle = 'rgba(0, 0, 0, 0.35)'
      ctx.fillRect(0, 0, c.width, c.height)

      for (const s of stars) {
        ctx.save()

        ctx.shadowColor = 'white'
        ctx.shadowBlur = s.v * 12

        ctx.globalAlpha = Math.min(s.v * 4, 1)

        ctx.beginPath()
        ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2)
        ctx.fillStyle = 'white'
        ctx.fill()

        ctx.restore()

        s.x += s.v
        s.y += s.v * 0.15

        if (s.x > c.width) {
          s.x = 0
          s.y = Math.random() * c.height
        }
      }

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
