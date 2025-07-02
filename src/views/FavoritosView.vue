<template>
  <div style="max-width: 800px; margin: 2rem auto">
    <h1>Mis Películas Favoritas</h1>

    <div v-if="favoritos.length === 0">
      <p>No agregaste ninguna película a favoritos.</p>
    </div>

    <ul v-else style="display: flex; flex-wrap: wrap; gap: 16px; padding: 0; list-style: none">
      <li v-for="peli in favoritos" :key="peli.id" style="width: 150px">
        <img
          :src="`https://image.tmdb.org/t/p/w300${peli.poster_path}`"
          :alt="peli.title"
          style="width: 100%; border-radius: 8px"
        />
        <h3 style="font-size: 1rem; margin: 8px 0 4px">{{ peli.title }}</h3>
        <p style="font-size: 0.9rem; color: #555">Estreno: {{ peli.release_date }}</p>

        <router-link
          :to="`/detalle/${peli.id}`"
          style="
            display: inline-block;
            margin-top: 6px;
            padding: 6px 10px;
            background: #007bff;
            color: white;
            border-radius: 4px;
            text-decoration: none;
          "
        >
          Ver detalle
        </router-link>

        <button
          @click="quitarFavorito(peli.id)"
          style="
            display: block;
            margin-top: 6px;
            padding: 6px 10px;
            background: #dc3545;
            color: white;
            border: none;
            border-radius: 4px;
            width: 100%;
          "
        >
          Quitar de favoritos
        </button>
      </li>
    </ul>
  </div>
  <div
    v-if="mostrarMensaje"
    style="
      position: fixed;
      top: 20px;
      right: 20px;
      background: #333;
      color: white;
      padding: 10px 16px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
      z-index: 1000;
      transition: opacity 0.3s;
    "
  >
    {{ mensaje }}
  </div>
</template>

<script setup>
import { ref, onMounted, onActivated } from 'vue'

const favoritos = ref([])
const mensaje = ref('')
const mostrarMensaje = ref(false)

const mostrarCartel = (texto) => {
  mensaje.value = texto
  mostrarMensaje.value = true
  setTimeout(() => {
    mostrarMensaje.value = false
  }, 2000) // se oculta después de 2 segundos
}

const cargarFavoritos = () => {
  favoritos.value = JSON.parse(localStorage.getItem('favoritos') || '[]')
}

const quitarFavorito = (id) => {
  const actualizados = favoritos.value.filter((p) => p.id !== id)
  localStorage.setItem('favoritos', JSON.stringify(actualizados))
  favoritos.value = actualizados
  mostrarCartel('Película quitada de favoritos')
}

onMounted(cargarFavoritos)
onActivated(cargarFavoritos)
</script>
