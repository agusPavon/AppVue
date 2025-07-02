<template>
  <v-img
    :src="heroImage"
    gradient="to top right, rgba(0,0,0,.7), rgba(0,0,0,.5)"
    height="400px"
    cover
    class="align-center justify-center text-white"
  >
    <v-container>
      <v-row justify="center">
        <v-col cols="12" md="8" class="text-center">
          <h1 class="text-h3 text-md-h2 mb-4">
            {{ !buscando ? 'Explora el mundo del cine' : 'Resultados de tu búsqueda' }}
          </h1>

          <div class="d-flex flex-column flex-md-row ga-3 justify-center">
            <v-text-field
              :model-value="busquedaValue"
              @update:model-value="$emit('update:busquedaValue', $event)"
              label="Buscar película por título..."
              variant="solo"
              density="comfortable"
              single-line
              hide-details
              bg-color="white"
              class="flex-grow-1"
            ></v-text-field>
            <v-select
              :model-value="generoSeleccionadoValue"
              @update:model-value="handleGeneroChange"
              :items="generosItems"
              item-title="name"
              item-value="id"
              label="Todos los géneros"
              variant="solo"
              density="comfortable"
              single-line
              hide-details
              bg-color="white"
              class="flex-grow-0"
            ></v-select>
            <v-btn @click="$emit('buscar')" color="primary" size="large"> Buscar </v-btn>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </v-img>
</template>

<script setup>
import { computed } from 'vue'
import bannerDefault from '@/assets/img/banner-1.png'
import bannerBuscando from '@/assets/img/banner-2.png'

const props = defineProps({
  buscando: Boolean,
  busquedaValue: String, // Usamos 'Value' para seguir el patrón de v-model
  generoSeleccionadoValue: String, // Usamos 'Value' para seguir el patrón de v-model
  generosItems: Array, // Usamos 'Items' para evitar conflicto con el ref 'generos' del padre
})

const emit = defineEmits(['update:busquedaValue', 'update:generoSeleccionadoValue', 'buscar']) // <-- ¡Descomenta o vuelve a poner esta línea!

const handleGeneroChange = (newValue) => {
  emit('update:generoSeleccionadoValue', newValue)
  emit('buscar') // Dispara la búsqueda después de actualizar el v-model
}

const heroImage = computed(() => {
  return props.buscando ? bannerBuscando : bannerDefault
})
</script>

<style scoped>
/* No necesitas muchos estilos aquí porque Vuetify hace la mayor parte */
/* Puedes añadir estilos específicos si quieres ajustar algo que Vuetify no cubra */
</style>
