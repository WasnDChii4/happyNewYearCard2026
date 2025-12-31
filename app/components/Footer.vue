<template>
  <footer class="footer text-lg font-bold font-finger-paint-regular" :class="{ show }">
    <span>By Wasn</span>
  </footer>
</template>

<script setup lang="ts">
  defineProps<{
    show: boolean
  }>()
</script>

<style scoped>
  .footer {
    position: fixed;
    bottom: 16px;
    left: 50%;
    transform: translateX(-50%) translateY(6px);
    opacity: 0;
    color: white;
    letter-spacing: 0.08em;

    transition: all 0.6s ease;
    z-index: 3;
    pointer-events: none;
  }

  .footer.show {
    opacity: 0.6;
    transform: translateX(-50%) translateY(0);
  }
</style>
