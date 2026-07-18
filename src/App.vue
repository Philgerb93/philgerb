<template>
  <div id="app">
    <NavBar />
    <HeroBanner />
    <div id="start-of-page"></div>
    <div class="main-content">
      <MyValues />
      <Assets />
      <ContactMe />
    </div>
    <div id="end-of-page"></div>
  </div>
</template>

<script setup>
import NavBar     from './components/NavBar.vue'
import HeroBanner from './components/HeroBanner.vue'
import MyValues   from './components/MyValues.vue'
import Assets     from './components/Assets.vue'
import ContactMe  from './components/ContactMe.vue'
</script>

<style lang="scss">
@use './base.scss' as *;
</style>
