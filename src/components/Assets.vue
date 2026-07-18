<template>
  <div class="assets" id="assets">
    <div class="page-section">
      <h2 class="with-subheader">Assets</h2>
      <p class="subheader">Battle-tested tools built by a software engineer, for Unity developers.</p>

      <div
        v-for="(asset, i) in assets"
        :key="asset.id"
        class="asset"
      >
        <!-- Image left on even, right on odd -->
        <div class="media left" :class="{ 'only-mobile': i % 2 !== 0 }">
          <h3 class="only-mobile">{{ asset.title }}</h3>
          <div class="image-wrapper">
            <img :src="asset.image" :alt="asset.title" />
          </div>
        </div>

        <div class="info" :class="i % 2 === 0 ? 'right' : 'left'">
          <h3 class="only-desktop">{{ asset.title }}</h3>
          <p>{{ asset.description }}</p>
          <div class="tags">
            <span v-for="tag in asset.tags" :key="tag" class="tag">{{ tag }}</span>
          </div>
          <a :href="asset.storeUrl" target="_blank" rel="noopener" class="store-link">
            View on Asset Store →
          </a>
        </div>

        <div v-if="i % 2 !== 0" class="media only-desktop right">
          <div class="image-wrapper">
            <img :src="asset.image" :alt="asset.title" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue'
import peakyCover from '../assets/PeakyCover.png'

const assets = [
  {
    id: 1,
    title: 'Peaky Stamina',
    description:
      'Peaky Stamina is a plug-and-play system designed to add customizable stamina, condition effects, and ready-to-use UI to your game in minutes.',
    tags: ['Tool', 'Built-in', 'URP'],
    image: peakyCover,
    storeUrl: 'https://u3d.as/44JY',
  },
]

function watchForAnim() {
  document.querySelectorAll('.asset .left, .asset .right').forEach(el => {
    if (!el.classList.contains('slided') && elemInViewport(el)) {
      el.classList.add('slided')
    }
  })
}

function elemInViewport(el) {
  const navH = document.querySelector('.nav')?.clientHeight ?? 0
  const top = el.getBoundingClientRect().top
  const center = top + el.clientHeight / 2
  const topSect = center - el.clientHeight / 2
  const botSect = center + el.clientHeight / 2
  return (
    (topSect > navH && topSect < window.innerHeight) ||
    (botSect > navH && botSect < window.innerHeight)
  )
}

onMounted(() => { watchForAnim(); window.addEventListener('scroll', watchForAnim) })
onUnmounted(() => window.removeEventListener('scroll', watchForAnim))
</script>

<style lang="scss" scoped>
@use 'sass:color';
@use '../global.scss' as *;

.assets {
  background-color: rgba($color-accent-dark, 0.03);
}

.asset {
  display: flex;
  flex-direction: column;
  margin-bottom: 6rem;

  &:last-child {
    margin-bottom: 0;
  }

  @include width-above(800px) {
    flex-direction: row;
    height: 400px;
    margin-bottom: 20rem;

    &:last-child {
      margin-bottom: 0;
    }
  }

  .left {
    opacity: 0;
    &.slided { animation: 1s slideInUp forwards; }
    @include width-above(800px) {
      margin-right: 5%;
      &.slided { animation: 1s slideInRight forwards; }
    }
  }

  .right {
    opacity: 0;
    &.slided { animation: 1s slideInUp forwards; }
    @include width-above(800px) {
      margin-left: 5%;
      &.slided { animation: 1s slideInLeft forwards; }
    }
  }

  .media {
    flex: 1;

    .image-wrapper {
      width: 100%;
      height: 200px;
      max-width: 550px;
      overflow: hidden;
      border-radius: 6px;
      margin: 0 auto;
      box-shadow: 0 16px 38px -12px rgba(0,0,0,.56),
                  0 4px 25px 0px rgba(0,0,0,.12),
                  0 8px 10px -5px rgba(0,0,0,.2);

      @include width-above(800px) { height: 400px; }

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        transition: 0.5s all;
        &:hover { transform: scale(1.05); }
      }
    }
  }

  .info {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-top: 1.6rem;

    @include width-below(800px) { max-width: 600px; margin: 0 auto; }
    @include width-above(800px) { margin-top: 0; padding: 0 4rem; }

    h3, p { text-align: start; margin-left: 0; }
    @include width-below(800px) { p { margin-top: 3rem; } }

    .tags {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin: 1.2rem 0;

      .tag {
        font-size: 1.2rem;
        padding: 4px 12px;
        border-radius: 20px;
        border: 1px solid $color-brand;
        color: $color-brand;
        font-family: 'Montserrat', sans-serif;
        font-weight: 500;
        letter-spacing: 0.05em;
      }
    }

    .store-link {
      color: $color-brand;
      font-size: 1.6rem;
      font-weight: bold;
      transition: 0.2s all;
      width: fit-content;
      &:hover { color: color.adjust($color-brand, $lightness: 10%); }
    }
  }
}

.only-desktop {
  display: none;
  @include width-above(800px) { display: block; }
}

.only-mobile {
  display: block;
  @include width-above(800px) { display: none; }
}
</style>