<template>
  <div class="wrapper">
    <div class="year">
      <AnimatedYear v-if="phase !== 'idle'" />
      <span v-else class="glow">2025</span>
    </div>

    <div class="after-year">
      <transition name="slide-fade">
        <div v-if="phase === 'finished'" class="text">
          <h1>🎉 Happy New Year</h1>
          <p>May your year be filled with success and happiness ✨</p>
        </div>
      </transition>
    </div>

    <button v-if="phase === 'idle'" class="tap" @click="$emit('start')">
      Tap Me!
    </button>
  </div>
</template>

<script setup lang="ts">
  defineProps<{
    phase: 'idle' | 'transition' | 'finished'
  }>()

  defineEmits(['start'])

  import AnimatedYear from '@/components/AnimatedYear.vue'
</script>

<style scoped>
  .wrapper {
    text-align: center;
    color: white;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 0.4rem;
  }

  .year {
    font-size: clamp(4rem, 10vw, 7rem);
    font-weight: 900;
    letter-spacing: 0.15em;
    color: #ffd700;
  }

  .glow,
  .year span {
    text-shadow:
      0 0 8px rgba(255, 215, 0, 0.6),
      0 0 20px rgba(255, 215, 0, 0.5),
      0 0 40px rgba(255, 200, 0, 0.4),
      0 0 60px rgba(255, 170, 0, 0.3);
    animation: glowPulse 3s ease-in-out infinite;
  }

  .after-year {
    margin-top: -0.3rem;
  }

  @keyframes glowPulse {
    0%, 100% {
      text-shadow:
        0 0 8px rgba(255, 215, 0, 0.6),
        0 0 20px rgba(255, 215, 0, 0.5),
        0 0 40px rgba(255, 200, 0, 0.4);
    }
    50% {
      text-shadow:
        0 0 14px rgba(255, 230, 100, 0.9),
        0 0 35px rgba(255, 215, 0, 0.8),
        0 0 70px rgba(255, 190, 0, 0.6);
    }
  }

  .text h1 {
    margin: 0;
    font-size: 2rem;
  }

  .text p {
    margin-top: 0.2rem;
    opacity: 0.85;
    font-size: 1rem;
  }

  .tap {
    margin-top: 1.5rem;
    padding: 0.75rem 2.5rem;
    border-radius: 999px;
    background: linear-gradient(135deg, #ffd700, #ffae00);
    border: none;
    font-size: 1.1rem;
    cursor: pointer;
    box-shadow: 0 0 20px rgba(255, 215, 0, 0.7);
  }

  .slide-fade-enter-active {
    transition: all 0.8s ease;
  }
  .slide-fade-enter-from {
    opacity: 0;
    transform: translateY(-16px);
  }
</style>
