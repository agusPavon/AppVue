<script setup>
import { RouterLink, RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header>
    <!-- <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" /> -->

    <div class="wrapper">
      <!-- <HelloWorld msg="You did it!" /> -->

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <!-- <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/listado">Listado</RouterLink>
        <RouterLink to="/cat">Cat</RouterLink>
        <RouterLink to="/cats">Cats</RouterLink>
        <RouterLink to="/cards">Cards</RouterLink> -->
        <RouterLink to="/favoritos">Favoritos</RouterLink>
      </nav>
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
        border-radius: 6px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        z-index: 1000;
        transition: opacity 0.3s ease;
      "
    >
      {{ mensaje }}
    </div>
  </header>

  <RouterView v-slot="{ Component }">
    <KeepAlive>
      <component :is="Component" />
    </KeepAlive>
  </RouterView>
</template>
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
