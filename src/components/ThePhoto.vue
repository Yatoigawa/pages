<script setup lang="ts">
import ImageFrame from './ImageFrame.vue'
</script>

<template>
  <div class="main">
    <ImageFrame img="pages/img/DSC06569.JPG" />
    <ImageFrame img="pages/img/DSC07281.JPG" />
    <ImageFrame img="pages/img/DSC00454.JPG" />
  </div>
</template>

<style scoped>
.main {
  padding: 1rem;
  display: flex;
  flex-flow: row wrap;
  gap: 2rem;
  justify-content: center;
}
</style>
