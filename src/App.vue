<script setup>
import axios from 'axios'
// import { ref, watchEffect } from 'vue'
import { reactive, watchEffect } from "vue"

const state = reactive({
 count: 1,
 name: 'Leo'
})

watchEffect(() => {
 // Runs immediately
 // Logs "Count: 0, Name: Leo"
 axios
    .get(`https://jsonplaceholder.typicode.com/posts/${state.count}`)
    .then((response) => {
      console.log(response.data.title)
    })
  console.log(state.name) 
})
</script>

<template>
  <meta name="theme-color" content="#FFFFFF" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.2/font/bootstrap-icons.min.css">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
  <button :style="{ 'visibility': state.name === 'home' ? 'visible' : 'hidden'}" type="button" class="btn btn-primary" @click="state.count++">
    <i class="bi bi-shop"></i>
  </button>
  <button @click="state.count++">{{ state.count }}</button>
  <nav class="navbar fixed-bottom bg-body-tertiary">
  <div class="container-fluid">
    <a class="navbar-brand" href="#" @click="state.name = 'home'">
      <i class="bi bi-house-fill"></i>
    </a>
    <a class="navbar-brand" href="#" @click="state.name = 'order'">
      <i class="bi bi-cart-check-fill"></i>
    </a>
    <a class="navbar-brand" href="#" @click="state.name = 'analitics'">
      <i class="bi bi-graph-up-arrow"></i>
    </a>
    <a class="navbar-brand" href="#" @click="state.name = 'inventory'">
      <i class="bi bi-box-seam-fill"></i>
    </a>
    <a class="navbar-brand" href="#" @click="state.name = 'chat'">
      <i class="bi bi-chat-dots-fill"></i>
    </a>
    <a class="navbar-brand" href="#" @click="state.name = 'profile'">
      <i class="bi bi-person-lines-fill"></i>
    </a>
  </div>
</nav>
</template>
<!-- <script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style> -->
