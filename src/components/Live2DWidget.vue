<script setup lang="ts">
import { onMounted, watch, onBeforeUnmount } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
let initInterval: number | null = null

onMounted(() => {
  // Load autoload.js if not present
  if (!document.querySelector('script[src="/live2d/autoload.js"]')) {
    const script = document.createElement('script')
    script.src = '/live2d/autoload.js'
    script.async = true
    document.body.appendChild(script)
  }

  // Poll initially until it appears
  initInterval = window.setInterval(() => {
    const waifu = document.getElementById('waifu')
    if (waifu) {
      updateVisibility()
      if (initInterval) {
        clearInterval(initInterval)
        initInterval = null
      }
    }
  }, 200)
})

const updateVisibility = () => {
  const waifu = document.getElementById('waifu')
  if (waifu) {
    // Hide on login and register pages
    if (route.name === 'login' || route.name === 'register') {
      waifu.style.display = 'none'
    } else {
      waifu.style.display = 'block'
    }
  }
}

// Watch for route changes
watch(
  () => route.name,
  () => {
    updateVisibility()
    // Retry in case of race conditions or delayed rendering
    setTimeout(updateVisibility, 100)
  },
  { immediate: true },
)

onBeforeUnmount(() => {
  if (initInterval) {
    clearInterval(initInterval)
  }
})
</script>

<template>
  <div style="display: none"></div>
</template>
