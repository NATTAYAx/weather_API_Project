<template>
  <header>
    <MenuBar />
  </header>
  <main>
    <TodayData />
    <ForecastData />
  </main>
</template>

<script setup>
import MenuBar from '@/components/MenuBar.vue'
import TodayData from '@/components/TodayData.vue'
import ForecastData from '@/components/ForecastData.vue'
</script>

<style scoped></style>
