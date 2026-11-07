<script setup>

import ScrollingHeader from './components/ScrollingHeader.vue';
import NavBar from './components/NavBar.vue'
import SiteFooter from './components/SiteFooter.vue'
</script>

<template>
  <ScrollingHeader />
  <NavBar />
  <RouterView />
  <SiteFooter />
</template>
