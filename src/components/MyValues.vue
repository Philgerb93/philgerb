<template>
  <div class="values page-section" id="values">
    <h2 class="with-subheader">My values</h2>
    <p class="subheader">Every asset I ship is built around three pillars.</p>
    <div class="wrapper">
      <div class="value" ref="v1">
        <!-- Quality icon -->
        <svg class="value-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M11.049 2.927c.3-.921 1.603-.921 1.902 0l1.519 4.674a1 1 0 00.95.69h4.915c.969 0 1.371 1.24.588 1.81l-3.976 2.888a1 1 0 00-.363 1.118l1.518 4.674c.3.922-.755 1.688-1.538 1.118l-3.976-2.888a1 1 0 00-1.176 0l-3.976 2.888c-.783.57-1.838-.197-1.538-1.118l1.518-4.674a1 1 0 00-.363-1.118l-3.976-2.888c-.784-.57-.38-1.81.588-1.81h4.914a1 1 0 00.951-.69l1.519-4.674z"/>
        </svg>
        <h3>Quality</h3>
        <p>I obsess over every detail — clean code, polished UI, thorough documentation. I don't release anything I wouldn't want to use for my own projects.</p>
      </div>
      <div class="value" ref="v2">
        <!-- Plug and play icon -->
        <svg class="value-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
          <path d="M9 2v5M15 2v5M6 8h12v3a6 6 0 0 1-6 6v5M12 17v5M9 14v-2M15 14v-2"/>
        </svg>
        <h3>Ease of Use</h3>
        <p>I build packages to be plug and play — easy to install, easy to use, and free of unnecessary hassle.</p>
      </div>
      <div class="value" ref="v3">
        <!-- Support icon -->
        <svg class="value-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M18.364 5.636a9 9 0 11-12.728 0M12 3v9"/>
        </svg>
        <h3>Support</h3>
        <p>I stand behind everything I publish. Timely updates, responsive support, and compatibility with the latest Unity LTS releases — guaranteed.</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue'

function watchForAnim() {
  document.querySelectorAll('.value').forEach(el => {
    if (!el.classList.contains('slided') && elemInViewport(el)) {
      el.classList.add('slided')
    }
  })
}

function elemInViewport(el) {
  const navH   = document.querySelector('.nav')?.clientHeight ?? 0
  const top    = el.getBoundingClientRect().top
  const center = top + el.clientHeight / 2
  return center > navH && center < window.innerHeight
}

onMounted(() => { watchForAnim(); window.addEventListener('scroll', watchForAnim) })
onUnmounted(() => window.removeEventListener('scroll', watchForAnim))
</script>

<style lang="scss" scoped>
@use 'sass:color';
@use '../global.scss' as *;

.wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

  .value {
    margin: 16px;
    max-width: 400px;
    opacity: 0;

    &.slided { animation: slideInUp 1s forwards; }

    &:hover .value-icon { color: color.adjust($color-brand, $lightness: 10%); }

    .value-icon {
      width: 60px;
      height: 60px;
      color: $color-brand;
      transition: 0.2s all;
    }

    @include width-above(820px) { flex: 1; }
  }
}
</style>
