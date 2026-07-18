<template>
  <nav id="nav" class="nav" :class="{ hidden: !showBar }">
    <div class="nav-wrapper">
      <a href="#start-of-page" class="nav-name">philgerb</a>

      <label class="nav-btn" @click="togglePopup">
        <span class="nav-icon" :class="{ open: showPopup }"></span>
      </label>

      <ul class="nav-items">
        <li><a class="nav-link" href="#values">Values</a></li>
        <li><a class="nav-link" href="#assets">Assets</a></li>
        <li><a class="nav-link" href="#contact">Support</a></li>
      </ul>
    </div>

    <div class="nav-popup-bg" :class="{ open: showPopup }"></div>
    <nav class="nav-popup" :class="{ open: showPopup }">
      <ul class="nav-popup-list">
        <li class="nav-popup-item"><a href="#values"  class="nav-popup-link" @click="togglePopup">Values</a></li>
        <li class="nav-popup-item"><a href="#assets"  class="nav-popup-link" @click="togglePopup">Assets</a></li>
        <li class="nav-popup-item"><a href="#contact" class="nav-popup-link" @click="togglePopup">Support</a></li>
      </ul>
    </nav>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const showBar   = ref(false)
const showPopup = ref(false)

function togglePopup() { showPopup.value = !showPopup.value }

function navScroll() {
  const mc = document.querySelector('.main-content')
  if (!mc) return
  showBar.value = mc.getBoundingClientRect().top <= 100
}

onMounted(() => {
  document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', e => {
      e.preventDefault()
      const target = anchor.getAttribute('href')
      const navH   = Math.floor(document.querySelector('.nav-wrapper')?.clientHeight ?? 0)
      if (target === '#start-of-page') { window.scrollBy({ top: 20, behavior: 'smooth' }); return }
      const dest = document.querySelector(target)
      if (dest) window.scrollBy({ top: dest.getBoundingClientRect().top - navH, behavior: 'smooth' })
    })
  })
  navScroll()
  window.addEventListener('scroll', navScroll)
})
onUnmounted(() => window.removeEventListener('scroll', navScroll))
</script>

<style lang="scss">
@use 'sass:color';
@use '../global.scss' as *;

.nav {
  background-color: $color-dark;
  color: $color-light;
  left: 0;
  line-height: $navbar-height;
  position: fixed;
  text-transform: uppercase;
  top: 0;
  width: 100%;
  z-index: 10;
  transition: all 0.4s;
  height: 48px;
  overflow: hidden;

  &.hidden { height: 0; }

  & a { color: $color-light; }

  &-name {
    font-size: 1.6rem;
    letter-spacing: 0.2em;
    font-weight: bold;
    position: relative;
  }

  &-link {
    letter-spacing: 0.08em;
    position: relative;
    font-size: 1.2rem;
  }

  &-name, &-link {
    &::before {
      background-color: $color-light;
      bottom: -2px;
      content: '';
      height: 1px;
      left: 0;
      position: absolute;
      transform: scaleX(0);
      transition: all 0.2s ease-in-out;
      visibility: hidden;
      width: 100%;
    }
    &:hover::before { transform: scaleX(1); visibility: visible; }
  }

  &-wrapper {
    margin: auto;
    max-width: 90%;
  }

  &-items {
    display: none;
    float: right;

    @include width-above(820px) { display: inline-block; }

    & a  { margin-left: 2.6em; }
    & li { float: left; list-style-type: none; }
  }

  &-popup {
    height: 100vh;
    width: 0;
    opacity: 0;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 11;

    &-bg {
      height: 3rem;
      width: 3rem;
      border-radius: 50%;
      position: fixed;
      top: -2.5rem;
      right: -2.5rem;
      z-index: 8;
      transition: transform 0.4s;
      background-color: $color-dark;
    }

    &-list {
      list-style: none;
      @include absCenter;
    }

    &-item { margin: 1rem; }

    &-link {
      display: block;
      font-size: 2rem;
      font-weight: 300;
      padding: 0.4rem 0;
      color: #fff;
      text-decoration: none;
      text-transform: uppercase;
      text-align: center;
      transition: all 0.4s;
      &:hover { transform: translateX(1rem); }
    }
  }

  &-btn {
    position: fixed;
    top: 0.4rem;
    right: 2rem;
    z-index: 12;
    border: none;
    width: 4rem;
    height: 4rem;
    text-align: center;
    cursor: pointer;

    @include width-above(820px) { display: none; }
  }

  &-icon {
    position: relative;
    display: inline-block;
    width: 2rem;
    height: 2px;
    background-color: $color-light;
    top: 50%;
    transition: all 0.2s;

    &.open { background-color: transparent; }

    &::before, &::after {
      content: '';
      position: absolute;
      left: 0;
      width: 2rem;
      height: 2px;
      background-color: $color-light;
      transition: all 0.2s;
    }
    &::before { top: -0.6rem; }
    &::after  { top:  0.6rem; }

    &.open::before { transform: rotate(45deg);  top: 0; }
    &.open::after  { transform: rotate(-45deg); top: 0; }
  }

  .open.nav-popup-bg { transform: scale(80); }

  .open.nav-popup {
    width: 100%;
    opacity: 1;
    background-color: $color-dark;
    transition: opacity 0.4s;
  }
}
</style>
