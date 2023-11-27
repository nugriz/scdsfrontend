<script setup>
import axios from 'axios'
// import { ref, watchEffect } from 'vue'
import { reactive, watchEffect } from "vue"

const state = reactive({
 count: 1,
 onLogin: true,
 loginerror: false,
 loading: false,
 onSignup: false,
 authorizd: false,
 token: ''
})

const shadow = ("box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;"
)
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
function sign(){
  state.loading = true
  const email = document.getElementById("exampleInputEmail1").value
  const password = document.getElementById("exampleInputPassword1").value
  const username = document.getElementById("exampleInputUsername1").value
  const url = state.onLogin ? 'https://ngestok-8ff9388ae0c0.herokuapp.com/users/auth' : 'https://ngestok-8ff9388ae0c0.herokuapp.com/users/'
  const data = {
    email: email,
    password: password,
    username: username,
  }
  // console.log(data)
  axios
    .post(url, JSON.stringify(data), {
        headers: {
          'Content-Type': 'application/json',
        }
      }
    )
    .then((response) => {
      state.loading = false
      // console.log(response.data)
      state.authorizd = true
      state.token = response.data.token
      // console.log(state.token)
    }, (err) => {
      state.loading = false
      console.log(err)
      state.loginerror = true
    }
  )
}

function moveToSignup(){
  state.onSignup = true
  state.onLogin = false
}
</script>

<template>
  <!-- HEADER -->
  <meta name="theme-color" content="#87CEEB" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.2/font/bootstrap-icons.min.css">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
  
  <!-- BODY -->

  <!-- LOADING-->
  <div :style="{ 'visibility': state.loading ? 'visible' : 'hidden'}" style="position: fixed; z-index: 1; left: 0; top: 0; width: 100vw; height: 100vh; text-align: center; padding-top: 40vh;">
    <div class="lds-ellipsis"><div></div><div></div><div></div><div></div></div>
  </div>
  
  <!-- LOGIN & SIGNUP-->
  <div class="card text-bg-light mx-auto" style="box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px; border-radius: 20px;" :style="{ 'display': state.authorizd ? 'none': ''}">
    <div class="card-header"><h1 style="margin: auto;">{{ state.onLogin ? "Login" : "Signup" }}</h1></div>
    <div class="card-body" style="padding-bottom: 0;">
      <div class="mb-3">
        <label for="exampleInputUsername1" class="form-label"><h6>Username</h6></label>
        <input type="username" class="form-control" id="exampleInputUsername1">
      </div>
      <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label"><h6>Email address</h6></label>
        <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
        <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
      </div>
      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label"><h6>Password</h6></label>
        <input type="password" class="form-control" id="exampleInputPassword1">
      </div>
      <button type="submit" class="w-100 btn" :style=shadow style="border-radius: 20px;" @click="sign()"><h5 style="margin: auto;">{{ state.onLogin ? "Log in" : "Sign up" }}</h5></button>
      <div class="mb-3">
        <hr>
        <div :style="{ 'visibility': state.loginerror ? 'visible' : 'hidden'}" style="color: red;">The data entered is incorrect</div>
        <div id="emailHelp" class="form-text">{{ state.onLogin ? "Does not have account? create right now" : "Already have account? Login now"}}</div>
        <button @click="moveToSignup()" class="w-100 btn mt-3" :style=shadow style="border-radius: 20px;"><h5 style="margin: auto;">{{ state.onLogin ? "Sign up" : "Log in"}}</h5></button>
      </div>
    </div>
  </div>

  <!-- HOME -->
  <div class="row row-cols-2 row-cols-md-2 g-4" :style="{ 'display': state.authorizd ? '' : 'none'}">
  <div class="col" style="padding: 5px; margin-top: 0;" v-for="n in 14" :key="n">
    <div class="card btn btn-light" style="border-radius: 5%; padding: 0;">
      <img src="https://placehold.jp/150x150.png" class="card-img-top" alt="..." style="border-top-left-radius: 5%; border-top-right-radius: 5%;">
      <div class="card-body" style="padding: 10%; padding-bottom: 0; text-align: left;">
        <h6 class="card-title">Emina Bright Stuff</h6>
        <b class="card-text">Rp22.000</b>
        <p>Emina Official Store</p>
      </div>
    </div>
  </div>
</div>

  <!-- <button :style="{ 'visibility': state.name === 'home' ? 'visible' : 'hidden'}" type="button" class="btn btn-primary" @click="state.count++">
    <i class="bi bi-shop"></i>
  </button>
  <button @click="state.count++">{{ state.count }}</button> -->
  <nav class="navbar fixed-bottom bg-body-tertiary" style="box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;">
  <div class="container-fluid" style="max-width: 90vw;">
    <a class="navbar" href="#" @click="state.name = 'home'">
      <i class="bi bi-house-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.name = 'order'">
      <i class="bi bi-cart-check-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.name = 'analitics'">
      <i class="bi bi-graph-up-arrow"></i>
    </a>
    <a class="navbar" href="#" @click="state.name = 'inventory'">
      <i class="bi bi-box-seam-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.name = 'chat'">
      <i class="bi bi-chat-dots-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.name = 'profile'">
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
