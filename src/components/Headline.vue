<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <h1 class="primary">{{ msg }}</h1>
</template>

<style scoped>
</style>
