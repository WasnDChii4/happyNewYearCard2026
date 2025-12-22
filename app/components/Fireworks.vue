<template>
  <canvas ref="canvas"></canvas>
</template>

<script setup lang="ts">
  import { ref, onMounted } from 'vue'

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

    const MAX_FIREWORKS = 5
    const MAX_PARTICLES = 800

    const colors = ['#ff004c', '#ffd700', '#00eaff', '#ffffff', '#7fff00']

    class Particle {
      x: number
      y: number
      vx: number
      vy: number
      alpha = 1
      radius: number
      color: string

      constructor(x: number, y: number, color: string) {
        const angle = Math.random() * Math.PI * 2
        const speed = Math.random() * 5 + 2

        this.x = x
        this.y = y
        this.vx = Math.cos(angle) * speed
        this.vy = Math.sin(angle) * speed
        this.radius = Math.random() * 1.5 + 1
        this.color = color
      }

      update() {
        this.vy += 0.02
        this.x += this.vx
        this.y += this.vy
        this.alpha -= 0.02
      }

      draw() {
        ctx.save()
        ctx.globalAlpha = this.alpha
        ctx.beginPath()
        ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2)
        ctx.fillStyle = this.color
        ctx.fill()
        ctx.restore()
      }
    }

    class Firework {
      x: number
      y: number
      vy: number
      targetY: number
      exploded = false
      color: string

      constructor() {
        this.x = Math.random() * c.width
        this.y = c.height
        this.vy = -(Math.random() * 4 + 6)
        this.targetY = Math.random() * c.height * 0.4 + 120
        this.color = colors[Math.floor(Math.random() * colors.length)]
      }

      update() {
        this.y += this.vy

        if (this.y <= this.targetY && !this.exploded) {
          this.explode()
          this.exploded = true
        }
      }

      explode() {
        const COUNT = 80

        for (let i = 0; i < COUNT; i++) {
          if (particles.length < MAX_PARTICLES) {
            particles.push(new Particle(this.x, this.y, this.color))
          }
        }
      }

      draw() {
        ctx.beginPath()
        ctx.arc(this.x, this.y, 2, 0, Math.PI * 2)
        ctx.fillStyle = this.color
        ctx.fill()
      }
    }

    const fireworks: Firework[] = []
    const particles: Particle[] = []

    let running = true
    document.addEventListener('visibilitychange', () => {
      running = !document.hidden
    })

    function animate() {
      if (!running) {
        requestAnimationFrame(animate)
        return
      }

      ctx.clearRect(0, 0, c.width, c.height)

      for (let i = fireworks.length - 1; i >= 0; i--) {
        const f = fireworks[i]
        f.update()
        if (!f.exploded) {
          f.draw()
        } else {
          fireworks.splice(i, 1)
        }
      }

      for (let i = particles.length - 1; i >= 0; i--) {
        const p = particles[i]
        p.update()
        p.draw()
        if (p.alpha <= 0) {
          particles.splice(i, 1)
        }
      }

      requestAnimationFrame(animate)
    }

    setInterval(() => {
      if (fireworks.length < MAX_FIREWORKS) {
        fireworks.push(new Firework())
      }
    }, 700)

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