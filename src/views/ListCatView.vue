<script setup>
import { ref } from 'vue'

let gatos = ref([])

fetch('https://api.thecatapi.com/v1/images/search?limit=10', {
  headers: {
    'x-api-key': 'live_hYuWIq2yoIF3b56tvcboIisNqkDNnjUiNPLN0PKFlsCcEPvJWhXuGUgt6gVSTROS',
  },
})
  .then((res) => res.json())
  .then((cats) => {
    gatos.value = cats
    console.log(gatos.value)
  })
</script>
<template>
  <h1>Listado de gatos</h1>
  <div v-if="gatos.length > 0">
    <v-container>
      <v-row>
        <v-col cols="12" sm="6" md="6" lg="6" v-for="gato in gatos">
          <v-img width="400px" :src="gato.url" :alt="gato.id" />
        </v-col>
      </v-row>
    </v-container>
  </div>
  <div v-else>
    <h2>Cargando....</h2>
  </div>
</template>

<style scoped></style>
