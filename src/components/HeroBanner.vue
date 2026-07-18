<template>
  <header class="header" id="header">
    <!-- <div class="header-bg"></div> -->
     <video width="1920" height="1080" autoplay muted loop playsinline>
      <source :src="bgVideo" type="video/mp4" />
    </video>
    <div class="wrapper">
      <h1>PHILGERB</h1>
      <p class="title">Unity Asset Store Publisher</p>
      <div class="buttons">
        <button @click="scrollTo('#assets')">Assets</button>
        <button @click="scrollTo('#contact')" class="outline">Support</button>
      </div>
      <div class="social">
        <a href="https://assetstore.unity.com/publishers/151978" target="_blank" rel="noopener" aria-label="Unity Asset Store" class="unity-link">
          <svg viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <path d="M10.928 4.291L4.695 14.5H2L12 20.918 22 14.5h-2.695L13.072 4.291zM12 17.5l-5.196-3 5.196-9 5.196 9z"/>
          </svg>
          <span>Unity Asset Store</span>
        </a>
      </div>
    </div>
  </header>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue'
import bgVideo from '../assets/bg.mp4'

function headerScroll() {
  const header    = document.querySelector('.header')
  const windowTop = window.pageYOffset || document.documentElement.scrollTop
  if (!header) return
  if (windowTop >= header.offsetHeight) {
    header.style.opacity = 0
  } else {
    header.style.opacity = 1
    const opacity = 1 - windowTop / header.offsetHeight
    document.querySelectorAll('.header .wrapper').forEach(el => el.style.opacity = opacity)
  }
}

function scrollTo(anchor) {
  const navH  = Math.floor(document.querySelector('.nav-wrapper')?.clientHeight ?? 0)
  const dest  = document.querySelector(anchor)
  if (dest) window.scrollBy({ top: dest.getBoundingClientRect().top - navH, behavior: 'smooth' })
}

onMounted(() => {
  headerScroll()
  window.addEventListener('scroll', headerScroll)
})
onUnmounted(() => window.removeEventListener('scroll', headerScroll))
</script>

<style lang="scss">
@use 'sass:color';
@use '../global.scss' as *;

.header {
  background-color: $color-dark;
  color: $color-light;
  height: 100vh;
  left: 0;
  position: fixed;
  top: 0;
  width: 100%;
  display: flex;
  align-items: center;

  @include height-above(800px) { height: $header-height; }

  .header-bg {
    background: linear-gradient(135deg, color.adjust($color-dark, $lightness: -4%) 0%, color.adjust($color-dark, $lightness: 6%) 100%);
    opacity: 0.9;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: -5;
    animation: videoAppear 0.8s forwards;
  }

  video {
    opacity: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    position: absolute;
    top: 0;
    left: 0;
    z-index: -5;
    animation: 0.4s videoAppear forwards;
  }

  .wrapper {
    display: flex;
    justify-content: center;
    flex-direction: column;
    margin: 0 auto;
    text-align: center;
    align-items: center;

    @include width-above(800px) {
      margin: 0;
      margin-left: 14vw;
      text-align: start;
      align-items: flex-start;
    }

    .title {
      font-size: 3rem;
      opacity: 0.8;
      margin-top: 0.4em;
    }

    .buttons {
      margin-top: 16px;

      button {
        background-color: $color-light;
        border: none;
        margin: 8px;
        width: 160px;
        height: 50px;
        font-weight: bold;
        text-transform: uppercase;
        color: $color-dark;
        transition: 0.2s all;

        &:hover { background-color: color.adjust($color-brand, $lightness: 10%); }

        &.outline {
          border: 1px solid $color-light;
          background-color: transparent;
          color: $color-light;

          &:hover {
            color: color.adjust($color-brand, $lightness: 10%);
            border-color: color.adjust($color-brand, $lightness: 10%);
          }
        }
      }
    }

    .social {
      border-top: 1px solid rgba($color-light, 0.5);
      width: 100%;
      max-width: 420px;
      padding-top: 16px;
      margin-top: 32px;
      display: flex;
      justify-content: flex-start;

      .unity-link {
        color: $color-light;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        gap: 1rem;
        width: 100%;
        min-height: 30px;
        padding: 0.4rem 0;
        transition: 0.2s all;
        animation: appear 0.4s forwards;
        opacity: 0;
        text-transform: uppercase;
        letter-spacing: 0.12em;
        font-size: 1.4rem;

        &:hover { color: color.adjust($color-brand, $lightness: 10%); }

        svg { width: 28px; height: 28px; flex: 0 0 auto; }
      }
    }
  }
}
</style>
