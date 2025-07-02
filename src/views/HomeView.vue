<template>
  <div>
    <HeroBanner
      :buscando="buscando"
      v-model:busqueda-value="busqueda"
      v-model:genero-seleccionado-value="generoSeleccionado"
      :generos-items="generosParaSelector"
      @buscar="buscarPeliculas"
    />
    <!-- Indicadores -->
    <p v-if="cargando">Cargando...</p>
    <p v-if="error" style="color: red">{{ error }}</p>
    <p v-if="!cargando && !error && sinResultados">No se encontraron películas.</p>

    <!-- Lista de películas -->
    <div
      class="lista-peliculas"
      v-if="!cargando && !error"
      style="display: flex; flex-wrap: wrap; gap: 16px"
    >
      <PeliculaCard v-for="peli in peliculas" :key="peli.id" :pelicula="peli">
        <p style="font-size: 0.9rem; color: #555; margin: 0">Estreno: {{ peli.release_date }}</p>
        <p style="font-size: 0.9rem; color: #555; margin: 0">
          Popularidad: {{ peli.popularity.toFixed(1) }}
        </p>
        <router-link
          :to="`/detalle/${peli.id}`"
          style="
            display: inline-block;
            margin-top: 6px;
            padding: 6px 10px;
            background: rgb(189, 62, 100);
            color: white;
            border-radius: 4px;
            text-decoration: none;
          "
          >Ver detalle</router-link
        >
      </PeliculaCard>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import PeliculaCard from '@/components/PeliculaCard.vue'
import HeroBanner from '@/components/HeroBanner.vue'

const apiKey = '44c94fabce903f582afb893ecf60bcd8'

const peliculas = ref([])
const generos = ref([])
const busqueda = ref('')
const generoSeleccionado = ref('')
const buscando = ref(false)

const cargando = ref(false)
const error = ref('')
const sinResultados = ref(false)

async function cargarGeneros() {
  const res = await fetch(
    `https://api.themoviedb.org/3/genre/movie/list?api_key=${apiKey}&language=es-ES`,
  )
  const data = await res.json()
  generos.value = data.genres
}

async function cargarPeliculasPopulares() {
  cargando.value = true
  error.value = ''
  sinResultados.value = false

  try {
    const res = await fetch(
      `https://api.themoviedb.org/3/movie/popular?api_key=${apiKey}&language=es-ES&page=1`,
    )
    const data = await res.json()
    peliculas.value = data.results
    buscando.value = false
  } catch (error) {
    error.value = 'No se pudieron cargar las películas populares.'
  } finally {
    cargando.value = false
  }
}

async function buscarPeliculas() {
  cargando.value = true
  error.value = ''
  sinResultados.value = false

  if (!busqueda.value.trim() && !generoSeleccionado.value) {
    await cargarPeliculasPopulares()
    return
  }

  buscando.value = true

  let url = ''

  if (busqueda.value.trim()) {
    url = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&language=es-ES&query=${encodeURIComponent(
      busqueda.value,
    )}`
  } else {
    url = `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&language=es-ES&with_genres=${generoSeleccionado.value}`
  }

  try {
    const res = await fetch(url)
    const data = await res.json()

    let resultados = data.results || []

    if (busqueda.value.trim() && generoSeleccionado.value) {
      resultados = resultados.filter((p) => p.genre_ids?.includes(Number(generoSeleccionado.value)))
    }

    peliculas.value = resultados
    sinResultados.value = resultados.length === 0
  } catch (error) {
    error.value = 'Error al buscar películas. Intentalo más tarde.'
  } finally {
    cargando.value = false
  }
}

const generosParaSelector = computed(() => {
  // Esta propiedad computada crea un nuevo array que incluye "Todos los géneros"
  // y luego los géneros puros de la API.
  return [{ id: '', name: 'Todos los géneros' }, ...generos.value]
})
onMounted(() => {
  cargarGeneros()
  cargarPeliculasPopulares()
})
</script>

<style scoped>
header {
  width: 100%;
  background-color: #111;
  padding: 1rem 0;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  z-index: 10;
}

nav {
  display: flex;
  gap: 1rem;
  justify-content: center;
  align-items: center;
}

nav a {
  color: white;
  text-decoration: none;
  font-weight: bold;
  font-size: 1rem;
  padding: 0.5rem 1rem;
  transition:
    background 0.3s,
    color 0.3s;
}

nav a:hover {
  background-color: #333;
  border-radius: 6px;
}

nav a.router-link-exact-active {
  color: #00ffb3;
}

@media (max-width: 600px) {
  nav {
    flex-direction: column;
  }
}
</style>
