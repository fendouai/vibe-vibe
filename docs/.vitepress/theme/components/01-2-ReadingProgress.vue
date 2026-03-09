<template>
  <Transition name="progress-fade">
    <button
      v-if="showProgress"
      type="button"
      class="reading-progress"
      :aria-label="buttonLabel"
      :title="buttonLabel"
      @click="scrollToTop"
    >
      <svg class="progress-ring" viewBox="0 0 56 56" aria-hidden="true">
        <circle class="progress-ring-bg" cx="28" cy="28" r="24" />
        <circle
          class="progress-ring-circle"
          cx="28"
          cy="28"
          r="24"
          :style="{ strokeDashoffset: circumference - (progress / 100) * circumference }"
        />
      </svg>
      <Transition name="content-switch">
        <span v-if="showArrow" key="arrow" class="progress-arrow" aria-hidden="true">↑</span>
        <span v-else key="percent" class="progress-text">{{ progress }}%</span>
      </Transition>
    </button>
  </Transition>
</template>

<script setup lang="ts">
import { computed, nextTick, onMounted, onUnmounted, ref, watch } from 'vue'
import { useRoute } from 'vitepress'

const route = useRoute()
const progress = ref(0)
const showProgress = ref(false)
const showArrow = ref(false)
const circumference = 2 * Math.PI * 24
let scrollTimer: number | null = null

const buttonLabel = computed(() =>
  route.path.startsWith('/en/')
    ? showArrow.value
      ? 'Back to top'
      : `Reading progress ${progress.value}%`
    : showArrow.value
      ? '返回顶部'
      : `阅读进度 ${progress.value}%`
)

const clearScrollTimer = () => {
  if (scrollTimer !== null) {
    window.clearTimeout(scrollTimer)
    scrollTimer = null
  }
}

const updateProgress = () => {
  const scrollTop = window.scrollY
  const docHeight = document.documentElement.scrollHeight - window.innerHeight
  const scrollPercent = docHeight > 0 ? (scrollTop / docHeight) * 100 : 0

  progress.value = Math.min(Math.max(Math.round(scrollPercent), 0), 100)
  showProgress.value = scrollTop > 0
  showArrow.value = false

  clearScrollTimer()

  scrollTimer = window.setTimeout(() => {
    if (window.scrollY > 0) {
      showArrow.value = true
    }
  }, 1500)
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

onMounted(() => {
  window.addEventListener('scroll', updateProgress, { passive: true })
  updateProgress()
})

watch(
  () => route.path,
  () => {
    nextTick(() => {
      clearScrollTimer()
      updateProgress()
    })
  }
)

onUnmounted(() => {
  window.removeEventListener('scroll', updateProgress)
  clearScrollTimer()
})
</script>

<style scoped>
.reading-progress {
  position: fixed;
  right: 28px;
  bottom: 28px;
  display: block;
  width: 56px;
  height: 56px;
  padding: 0;
  border: 0;
  border-radius: 999px;
  background: transparent;
  cursor: pointer;
  z-index: 120;
  color: inherit;
  filter: drop-shadow(0 8px 22px rgba(20, 20, 19, 0.12));
  transition: transform 0.25s ease, filter 0.25s ease;
  -webkit-tap-highlight-color: transparent;
}

:global(.dark) .reading-progress {
  filter: drop-shadow(0 10px 24px rgba(0, 0, 0, 0.28));
}

.reading-progress:hover {
  transform: translateY(-2px);
}

.reading-progress:active {
  transform: scale(0.97);
}

.reading-progress:focus {
  outline: none;
}

.reading-progress:focus-visible {
  outline: 2px solid var(--vp-c-text-1);
  outline-offset: 4px;
}

.progress-ring {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  transform: rotate(-90deg);
}

.progress-ring-bg {
  fill: var(--vp-c-bg);
  stroke: color-mix(in srgb, var(--vp-c-text-2) 16%, transparent);
  stroke-width: 3;
}

.progress-ring-circle {
  fill: none;
  stroke: var(--vp-c-text-1);
  stroke-width: 3;
  stroke-linecap: round;
  stroke-dasharray: 150.796;
  transition: stroke-dashoffset 0.3s ease;
}

.progress-text,
.progress-arrow {
  position: absolute;
  top: 50%;
  left: 50%;
  pointer-events: none;
  user-select: none;
  transform: translate(-50%, -50%);
}

.progress-text {
  font-size: 11px;
  font-weight: 600;
  line-height: 1;
  color: var(--vp-c-text-1);
}

.progress-arrow {
  font-size: 22px;
  font-weight: 600;
  line-height: 1;
  color: var(--vp-c-text-1);
  animation: bounce 1s ease-in-out infinite;
}

@keyframes bounce {
  0%,
  100% {
    transform: translate(-50%, -50%);
  }

  50% {
    transform: translate(-50%, -60%);
  }
}

.content-switch-enter-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.content-switch-leave-active {
  transition: opacity 0.15s ease, transform 0.15s ease;
}

.content-switch-enter-from {
  opacity: 0;
  transform: translate(-50%, -42%) scale(0.84);
}

.content-switch-leave-to {
  opacity: 0;
  transform: translate(-50%, -58%) scale(0.84);
}

.progress-fade-enter-active,
.progress-fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.progress-fade-enter-from,
.progress-fade-leave-to {
  opacity: 0;
  transform: scale(0.85) translateY(8px);
}

@media (max-width: 768px) {
  .reading-progress {
    right: 20px;
    bottom: 20px;
    width: 48px;
    height: 48px;
  }

  .progress-text {
    font-size: 10px;
  }

  .progress-arrow {
    font-size: 18px;
  }
}
</style>
