<template>
  <slot v-if="isDev" />
</template>

<script setup>
// 1. Definisikan variabel 'isDev' di dalam script
const isDev = import.meta.env.DEV
</script>

<style scoped>
/* style tidak perlu diubah */
</style>