<template>
  <main id="app">
    <MainPage></MainPage>
  </main>
</template>

<script>
import MainPage from "@/components/MainPage";
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    user: "Anonimus666",
    roomCurrent: null,
    FORBIDDEN_SYMBOLS: new RegExp("^[`~@\"#$%^&?*/';:]")
  },
  mutations: {
    auth (state, user) {
      state.user = user
    },
    changeRoom (state, room) {
      state.roomCurrent = room
    }
  }
})

export default {
  name: 'App',
  components: {
    MainPage
  },
  store
}
</script>

<style>
  main {
    font-family: "Segoe Print", "Times New Roman", sans-serif;
    font-size: 18px;
    padding: 100px 0;

    background-image: url("assets/back.jpg");
    background-position: 0 0;
    background-repeat: no-repeat;
    background-size: 100% 100%;
  }

  h1, h2, ul, li {
    margin: 0;
    padding: 0;
  }
  ul {
    list-style: none;
  }
  button {
    font-family: inherit;
    font-size: 24px;

    background-color: transparent;
  }

  input, textarea {
    font-family: inherit;
    background-color: #e6e6fa;
  }

  button, input, textarea {
    border: none;
    border-radius: 20px;

  }

  button {
    background-color: #A17AD7;
    box-shadow: 2px 2px 2px 0 grey;
  }

  textarea, input {
    font-size: inherit;
    padding: 10px;
  }

  button:focus, textarea:focus, input:focus {
    outline: none;
  }

  button:active {
    background-color: #42b983;
  }

</style>
