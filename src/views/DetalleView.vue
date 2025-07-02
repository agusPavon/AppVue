<template>
  <div style="max-width: 90%; margin: 2rem">
    <router-link to="/" style="display: inline-block; margin-bottom: 1rem">← Volver</router-link>

    <div v-if="pelicula" class="movie-detail-content">
      <div class="movie-detail">
        <img
          v-if="pelicula.poster_path"
          :src="`https://image.tmdb.org/t/p/w300${pelicula.poster_path}`"
          :alt="pelicula.title"
          class="movie-poster-float"
        />

        <h1>{{ pelicula.title }}</h1>

        <p><strong>Duración:</strong> {{ pelicula.runtime }} minutos</p>
        <p><strong>Géneros:</strong> {{ nombresGeneros }}</p>
        <p><strong>Popularidad:</strong> {{ pelicula.popularity }}</p>
        <p><strong>Fecha de estreno:</strong> {{ pelicula.release_date }}</p>

        <p>{{ pelicula.overview }}</p>

        <button
          @click="toggleFavorito"
          style="
            padding: 10px 16px;
            border: none;
            background: #ff5722;
            color: white;
            border-radius: 4px;
            margin-top: 1rem;
          "
        >
          {{ esFavorita ? 'Quitar de favoritos' : 'Agregar a favoritos' }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onActivated } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const apiKey = '44c94fabce903f582afb893ecf60bcd8'
const pelicula = ref(null)
const esFavorita = ref(false)
const mensaje = ref('')
const mostrarMensaje = ref(false)

const mostrarToast = (texto) => {
  mensaje.value = texto
  mostrarMensaje.value = true

  setTimeout(() => {
    mostrarMensaje.value = false
  }, 2500) // el mensaje se oculta después de 2.5 segundos
}

const cargarPelicula = async () => {
  const id = route.params.id
  const res = await fetch(
    `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&language=es-ES`,
  )
  const data = await res.json()
  pelicula.value = data
  verificarFavorito()
}

const verificarFavorito = () => {
  const favoritos = JSON.parse(localStorage.getItem('favoritos') || '[]')
  esFavorita.value = favoritos.some((p) => p.id === pelicula.value.id)
}

const toggleFavorito = () => {
  const favoritos = JSON.parse(localStorage.getItem('favoritos') || '[]')

  if (esFavorita.value) {
    const actualizados = favoritos.filter((p) => p.id !== pelicula.value.id)
    localStorage.setItem('favoritos', JSON.stringify(actualizados))
    esFavorita.value = false
    mostrarToast('Película quitada de favoritos')
  } else {
    favoritos.push({
      id: pelicula.value.id,
      title: pelicula.value.title,
      poster_path: pelicula.value.poster_path,
      release_date: pelicula.value.release_date,
    })
    localStorage.setItem('favoritos', JSON.stringify(favoritos))
    esFavorita.value = true
    mostrarToast('Película agregada a favoritos')
  }
}

const nombresGeneros = computed(() => {
  if (!pelicula.value || !pelicula.value.genres) return ''
  return pelicula.value.genres.map((g) => g.name).join(', ')
})

onActivated(() => {
  cargarPelicula()
})
</script>

<style scoped>
/* ... (your existing styles) ... */

.movie-detail-content {
  max-width: 80%;

  /* This ensures the content wraps correctly around the floated image */
  overflow: hidden;
}

.movie-poster-float {
  float: left;
  width: 30%;
  max-width: 50%;
  margin-right: 20px;
  margin-bottom: 15px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(255, 255, 255, 0.562);
}

.container {
  max-width: 600px;
  margin: 2rem auto;
  padding: 0 1rem;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
}

.back-link {
  display: inline-block;
  margin-bottom: 1.5rem;
  color: #00e413;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;
}

.back-link:hover {
  color: #31f04b;
  text-decoration: underline;
}

.movie-detail {
  width: 90%;
  background: #353535;
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(255, 253, 253, 0.747);
  margin: 0 auto;
}

.movie-poster {
  width: 50%;

  border-radius: 12px;
  display: inline-block;
  margin-bottom: 1rem;
  box-shadow: 0 6px 12px rgb(0 0 0 / 0.15);
  transition: transform 0.3s ease;
}

.movie-poster:hover {
  transform: scale(1.05);
}

.movie-title {
  margin-bottom: 1rem;
  font-weight: 700;
  color: #222;
}

.overview {
  margin: 1.5rem 0;
  line-height: 1.6;
  font-size: 1rem;
  color: #555;
}

.favorite-btn {
  padding: 12px 20px;
  border: none;
  background-color: #ff5722;
  color: #fff;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition:
    background-color 0.3s ease,
    box-shadow 0.3s ease;
}

.favorite-btn:hover {
  background-color: #e64a19;
  box-shadow: 0 4px 12px rgb(255 87 34 / 0.5);
}

.movie-detail-content {
  font-size: 1rem;
}
</style>
