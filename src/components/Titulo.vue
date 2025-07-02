<script setup>
defineProps({
  texto: String,
  required: false,
})
</script>
<template>
  <h1>{{ texto }}</h1>
</template>

<style scoped></style>
