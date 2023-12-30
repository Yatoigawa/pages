<script setup lang="ts">
/**
 * コンポーネントプロパティ
 */
defineProps<{
  msg: string
}>()
</script>

<template>
  <div class="common-layout">
    <el-text class="mx-1" type="primary">{{ msg }}</el-text>
  </div>
</template>

<style scoped>
.common-layout {
  text-align: center;
}
</style>
