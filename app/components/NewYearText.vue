<template>
  <div class="wrapper">
    <transition
      name="dip-black"
      @after-enter="onDipFullyBlack"
      @after-leave="onDipFinished"
    >
      <div v-if="showDip" class="dip-overlay"></div>
    </transition>

    <div class="year" v-if="!hideInitialContent">
      <span class="glow">2025</span>
    </div>

    <div class="year" v-if="phase !== 'idle' && hideInitialContent">
      <AnimatedYear />
    </div>

    <div class="after-year">
      <transition name="slide-fade">
        <div v-if="phase === 'finished'" class="text">
          <h1 class="font-bold font-bbh-bartle-regular">Happy New Year</h1>
          <p class="font-henny-penny-regular">✨ May your year be filled with success and happiness ✨</p>
        </div>
      </transition>
    </div>

    <button v-if="phase === 'idle' && !hideInitialContent" class="tap text-black font-bold font-caveat-brush-regular text-3xl" @click="handleTap">
      Tap Me!
    </button>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import AnimatedYear from '@/components/AnimatedYear.vue'

  defineProps<{
    phase: 'idle' | 'transition' | 'finished'
  }>()

  const emit = defineEmits(['start'])

  const showDip = ref(false)
  const hideInitialContent = ref(false)

  function handleTap() {
    showDip.value = true
    setTimeout(() => {
      showDip.value = false
    }, 600)
  }

  function onDipFullyBlack() {
    hideInitialContent.value = true
  }

  function onDipFinished() {
    emit('start')
  }
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
    position: relative;
    z-index: 2;
  }

  .dip-overlay {
    position: fixed;
    inset: 0;
    background: radial-gradient(circle, #000, #000);
    z-index: 999;
    pointer-events: none;
  }

  .dip-black-enter-active {
    transition: opacity 0.4s ease;
  }
  .dip-black-enter-from {
    opacity: 0;
  }
  .dip-black-enter-to {
    opacity: 1;
  }

  .dip-black-leave-active {
    transition: opacity 0.9s ease;
  }
  .dip-black-leave-from {
    opacity: 1;
  }
  .dip-black-leave-to {
    opacity: 0;
  }

  .year {
    font-size: clamp(4rem, 10vw, 7rem);
    font-weight: 900;
    letter-spacing: 0.15em;
    color: #ffd700;
  }

  .glow {
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
    margin-top: 1rem;
    padding: 0.75rem 2.5rem;
    border-radius: 25px;
    background: linear-gradient(135deg, #ffd700, #ffae00);
    border: none;
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
