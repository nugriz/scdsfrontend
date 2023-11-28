<script setup>
import axios from 'axios'
// import { ref, watchEffect } from 'vue'
import { reactive, watchEffect } from "vue"

const state = reactive({
 page: 'home',
 count: 1,
 onLogin: true,
 loginerror: false,
 loading: false,
 onSignup: false,
 authorizd: false,
 token: '',
 products: [],
 supplier: [],
 namauser: '',
 emailuser: '',
 warehouses: [1,2,3,4],
 onCreateWarehouse: false
})

const shadow = ("box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;"
)
// watchEffect(() => {
//  // Runs immediately
//  // Logs "Count: 0, Name: Leo"
//  axios
//     .get(`https://jsonplaceholder.typicode.com/posts/${state.count}`)
//     .then((response) => {
//       console.log(response.data.title)
//     })
//   console.log(state.page) 
// })

function home(){
  state.loading = true
  const url = 'https://ngestok-8ff9388ae0c0.herokuapp.com/products'
  const tokens = 'Bearer ' + state.token
  const config = {
          'authorization': tokens,
          'content-type': 'application/json',
        }
  axios
    .get(url, {
        headers: config
      }
    )
    .then((response) => {
      state.loading = false
      console.log(response.data)
      state.authorizd = true
      state.products = response.data
      for (let index = 0; index < response.data.length; index++) {
        state.loading = false
        getSupplier(response.data[index].supplier_id)
      }
      // console.log(state.token)
    }, (err) => {
      state.loading = false
      console.log(config)
      state.loading = false
      console.log(err)
      state.loginerror = true
    }
  )
}

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
      console.log(state.onLogin)
      console.log(response.data)
      if (state.onLogin) {
        state.authorizd = true
        state.token = response.data.token
        state.namauser = username
        state.emailuser = email
        home()
      } else {
        state.onLogin = true
      }
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

const getSupplier = (id) => {
  state.loading = true
  const url = 'https://ngestok-8ff9388ae0c0.herokuapp.com/users/' + id 
  axios
    .get(url, {
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer ' + state.token
        }
      }
    ).then((response) =>  {
        state.loading = false
        console.log(url)
        state.supplier.unshift(response.data.username)
        console.log(response)
      }
    )
}

function createWarehouse(){
  state.onCreateWarehouse = true
}

function addWarehouse(){
  state.onCreateWarehouse = false

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
      <button type="submit" class="w-100 btn" style="border-radius: 20px; border-color: skyblue;" @click="sign()"><h5 style="margin: auto;">{{ state.onLogin ? "Log in" : "Sign up" }}</h5></button>
      <div class="mb-3">
        <hr :style="{ 'display': state.onSignup ? 'none': ''}">
        <div :style="{ 'visibility': state.loginerror ? 'visible' : 'hidden'}" style="color: red;">The data entered is incorrect</div>
        <div id="emailHelp" class="form-text" :style="{ 'display': state.onSignup ? 'none': ''}">{{ state.onLogin ? "Does not have account? create right now" : "Already have account? Login now"}}</div>
        <button @click="moveToSignup()" class="w-100 btn mt-3" :style="{ 'display': state.onSignup ? 'none': ''}" style="border-radius: 20px; border-color: skyblue;"><h5 style="margin: auto;">Sign up</h5></button>
      </div>
    </div>
  </div>

  <!-- HOME -->
  <div :style="{ 'display': state.authorizd ? '' : 'none'}" style="position: fixed; background-color: skyblue; left: 0; top: 0; z-index: 1;width: 100vw; height: 9vh; border-color: black; color: white; text-align: left; padding: 0.5vh; padding-left: 5%;">
    <i class="bi bi-bag-heart-fill" style="color: white;font-size: 2em;display: inline-block;"> </i>
    <div style="width: 2vw; display: inline-block;"></div>
    <h1 style="display: inline-block;">Supplify.id</h1>
  </div>
  <div style="height: 9vh;"></div>

  <div class="row row-cols-2 row-cols-md-2 g-4" :style="{ 'display': state.authorizd ? (state.page === 'home' ? '' : 'none') : 'none'}">
  <div class="col" style="padding: 5px; margin-top: 0;" v-for="product in state.products" :key="product">
    <div class="card btn btn-light" style="border-radius: 0.5em; padding: 0; border-color: white;">
      <img src="https://dummyimage.com/640x640/fff/aaa" class="card-img-top" alt="..." style="border-top-left-radius: 0.5em; border-top-right-radius: 0.5em;">
      <div class="card-body" style="padding: 10%; padding-bottom: 0; text-align: left;">
        <h6 class="card-title">{{ product.name }}</h6>
        <b class="card-text">Rp{{ product.price.toLocaleString() }}</b>
        <p>{{ state.supplier[-1] ? state.supplier.pop() : "Johnny & Johnson (J&J) "}}</p>
      </div>
    </div>
  </div>
  <div style="height: 1vh;"></div>
</div>

<!-- ORDERS-->
<div :style="{ 'display': state.page === 'order' ? '' : 'none'}"><h1 style="text-align: center; color: white;">Belum ada pesanan</h1></div>

<!-- ANALITICS-->
<div :style="{ 'display': state.page === 'analitics' ? '' : 'none'}"><h1 style="text-align: center; color: white;">Fitur ini belum tersedia</h1></div>

<!-- INVENTORI -->
<div :style="{ 'display': state.page === 'inventory' ? (state.onCreateWarehouse ? 'none' : '') : 'none'}">
  <button type="button" class="btn btn-primary btn-sm float-end" :style=shadow @click="createWarehouse()" style="width: 40vw;border-color: #D27D2D;background-color: #D27D2D;border-radius: 50px;"><h5 style="padding: 0.01%;">+ warehouse</h5></button>
  <button type="button" class="btn btn-primary btn-sm float-start" :style=shadow style="width: 40vw; border-color: indigo;background-color: indigo;border-radius: 50px;"><h5 style="padding: 0.01%;">+ produk mu   </h5></button>
  
  <div style="text-align: center; padding-top: 20vh;">
    <i v-if="!state.warehouses" class="bi bi-box2" style="color: skyblue; font-size: 1500%;"></i>

    <div><h1>Produk Jualan Mu</h1><hr></div>
    <div class="scrolling-wrapper">
      <div v-for="warehouse in state.warehouses" :key="warehouse" class="kartu card text-bg-light mb-3 btn btn-light" style="padding: 0;width: 8rem; text-align: left;">
        <img src="https://placehold.jp/150x150.png" class="card-img-top">
        <div class="card-body" style="overflow: hidden;">
          <h5 class="card-title">Light card title</h5>
          <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
        </div>
      </div>
    </div>
    <div style=" position: relative">
      <div style="height: 1vh;"></div>
      <h1>Daftar Warehouse</h1>
      <hr>
      <div class="d-grid gap-2">
        <button :style=shadow class="btn btn-light" type="button">Button</button>
        <button :style=shadow class="btn btn-light" type="button">Button</button>
        <button :style=shadow class="btn btn-light" type="button">Button</button>
        <button :style=shadow class="btn btn-light" type="button">Button</button>
      </div>
      <div style="height: 10vh;"></div>
    </div>
  </div>
</div>
<!-- FORM CREATE WAREHOUSE-->
<div class="card text-bg-light mx-auto" style="box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px; border-radius: 20px;" :style="{ 'display': state.onCreateWarehouse ? '': 'none'}">
  <div class="card-header"><h1 style="margin: auto;">Add Warehouse</h1></div>
  <div class="card-body" style="padding-bottom: 0;">
    <div class="mb-3">
      <label for="exampleInputLocation1" class="form-label"><h6>Location</h6></label>
      <input type="username" class="form-control" id="exampleInputLocation1">
    </div>
    <button type="submit" class="w-100 btn" style="border-radius: 20px; border-color: skyblue;" @click="addWarehouse()"><h5 style="margin: auto;">Add</h5></button>
    <div style="height: 2vh;"></div>
  </div>
</div>
<!-- CHAT -->
<div :style="{ 'display': state.page === 'chat' ? '' : 'none'}"><h1 style="text-align: center; color: white;">Belum ada percakapan</h1></div>

<!-- PROFIL -->
<div :style="{ 'display': state.authorizd ? (state.page === 'profile' ? '' : 'none') : 'none'}">
<div style="background-color: skyblue; position: fixed; left: 0; top: 8vh; width: 100vw; padding: 2em;">
  <i class="bi bi-person-circle" style="color: white;font-size: 4em;display: inline-block;" ></i>
  <div style="width: 6vw; display: inline-block;"></div>
  <h1 style="display: inline-block; font-size: 4em; color: white;">{{ state.namauser }}</h1>
</div>
<h3 style="position: fixed; left: 0; top: 27vh;padding: 1.5em;">email : {{ state.emailuser }}</h3>
<hr style="position: fixed; left: 0; top: 37vh;">
</div>

<!-- BOTTOM NAVIGATION BAR-->
<nav class="navbar fixed-bottom bg-body-tertiary" style="box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;">
  <div class="container-fluid" style="max-width: 90vw;">
    <a class="navbar" href="#" @click="state.page = 'home'">
      <i class="bi bi-house-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.page = 'order'">
      <i class="bi bi-cart-check-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.page = 'analitics'">
      <i class="bi bi-graph-up-arrow"></i>
    </a>
    <a class="navbar" href="#" @click="state.page = 'inventory'">
      <i class="bi bi-box-seam-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.page = 'chat'">
      <i class="bi bi-chat-dots-fill"></i>
    </a>
    <a class="navbar" href="#" @click="state.page = 'profile'">
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
