<template>
  <div
    class="pelicula-card"
    style="
      width: 150px;
      border: 1px solid #ccc;
      border-radius: 8px;
      padding: 8px;
      width: 100%;
      max-width: 200px;
    "
  >
    <img
      :src="`https://image.tmdb.org/t/p/w300${pelicula.poster_path}`"
      :alt="pelicula.title"
      style="width: 100%; border-radius: 6px"
    />
    <h3 style="font-size: 1rem; margin: 8px 0 4px">{{ pelicula.title }}</h3>

    <!-- Aquí va el contenido dinámico que enviás desde afuera -->
    <slot></slot>
  </div>
</template>

<script setup>
defineProps({
  pelicula: Object,
})
</script>
<style scoped>
.pelicula-card {
  transition:
    transform 0.3s ease,
    box-shadow 0.3s ease;
  box-shadow: 0 2px 6px rgba(255, 255, 255, 0.712);
  border-radius: 8px;
  overflow: hidden;
}

.pelicula-card:hover {
  transform: scale(1.05); /* Agranda un poco */
  box-shadow: 0 8px 20px rgba(233, 56, 159, 0.4); /* Sombra más intensa */
  z-index: 2;
}
</style>
