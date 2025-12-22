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

    class Particle {
      x: number
      y: number
      vx: number
      vy: number
      alpha: number
      color: string

      constructor(x: number, y: number, color: string) {
        this.x = x
        this.y = y
        this.vx = (Math.random() - 0.5) * 6
        this.vy = (Math.random() - 0.5) * 6
        this.alpha = 1
        this.color = color
      }

      draw() {
        ctx.save()
        ctx.globalAlpha = this.alpha
        ctx.beginPath()
        ctx.arc(this.x, this.y, 2, 0, Math.PI * 2)
        ctx.fillStyle = this.color
        ctx.fill()
        ctx.restore()
      }

      update() {
        this.x += this.vx
        this.y += this.vy
        this.alpha -= 0.015
      }
    }

    let particles: Particle[] = []

    function createFirework() {
      const x = Math.random() * c.width
      const y = Math.random() * c.height * 0.5
      const colors = ['#ff004c', '#ffd700', '#00eaff', '#ffffff']

      for (let i = 0; i < 50; i++) {
        particles.push(new Particle(x, y, colors[Math.floor(Math.random() * colors.length)]))
      }
    }

    function animate() {
      ctx.clearRect(0, 0, c.width, c.height)

      particles.forEach((p, i) => {
        p.update()
        p.draw()
        if (p.alpha <= 0) particles.splice(i, 1)
      })

      requestAnimationFrame(animate)
    }

    setInterval(createFirework, 1200)
    animate()
  })
</script>

<style scoped>
  canvas {
    position: fixed;
    inset: 0;
    z-index: 1;
    pointer-events: none;
  }
</style>
