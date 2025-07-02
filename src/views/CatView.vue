<script setup>
import { ref } from 'vue'
let gato = ref({})

function cambiarGato() {
  // fetch('https://api.thedogapi.com/v1/images/search')
  fetch('https://api.thecatapi.com/v1/images/search')
    .then((res) => res.json())
    .then((cat) => {
      gato.value = cat[0]
      console.log(gato.value)
    })
}

cambiarGato()
</script>
<template>
  <h1>Cat info</h1>
  <button @click="cambiarGato">Cambiar foto</button>
  <img :src="gato.url" :alt="gato.id" width="300px" />
</template>
