<template>
  <TheNavbar :myUser="myUser" />
  <TheHeader :myUser="myUser" :myUserNAME="myUserNAME" />
  <main class="container">
    <router-view />
  </main>
</template>

<script>
import { ref, provide } from 'vue'
import { onAuthStateChanged } from '@firebase/auth'
import { auth } from './firebase'
import TheNavbar from './components/TheNavbar.vue'
import TheHeader from './components/TheHeader.vue'
export default {
  components: { TheNavbar, TheHeader },
  setup () {
    const myUser = ref(auth.currentUser)
    const myUserNAME = ref('')

    // Si el estado del usuario cambia...
    onAuthStateChanged(auth, (user) => {
      myUser.value = user
    })
    provide('myUser', myUser)
    provide('myUserNAME', myUserNAME)

    return { myUser, myUserNAME }
  }
}
</script>

<style>
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
  padding: 0;
  margin: 0;
}
body {
  font-family: 'Poppins', sans-serif;
  background-color: #ebecf2;
}
body.dark-mode {
  background-color: #1a1e25;
  color: #fff;
}
#app {
  height: 100vh;
  min-height: 100vh;
  display: grid;
  grid-template-areas:
    'nav head head'
    'nav main main';
  align-items: flex-start;
  justify-content: flex-start;
}
.container {
  height: calc(100vh - 70px);
  min-height: calc(100vh - 70px);
  width: 100%;
  padding: 32px;
  grid-area: main;
}
.max-width {
  position: relative;
  max-width: 870px;
  min-height: 523px;
  margin: auto;
  margin-top: 20px;
  padding: 35px;
  border-radius: 20px;
  background-color: #fff;
}
body.dark-mode .max-width {
  background-color: #1f2633;
}
.title {
  font-size: 30px;
  line-height: 37px;
  /* margin-bottom: 10px; */
}
.btn {
  font-size: 17px;
  padding: 12px 40px;
  background-color: #008fff;
  border-radius: 20px;
  border: none;
  color: #ffffff;
  text-decoration: none;
  position: relative;
  cursor: pointer;
}
.btn::before {
  content: '';
  position: absolute;
  inset: 7px 4px;
  width: 100%;
  height: 100%;
  border-radius: 20px;
  background-image: linear-gradient(185deg,#09c0fa, #1443ede0);
  filter: blur(10px);
  opacity: 0.65;
  z-index: -1;
}

@media (max-width: 670px) {
  .header {
    width: 100vw;
  }
  .header__container {
    padding-left: 18px;
    padding-right: 18px;
  }
  .header__title {
    font-size: 24px;
  }
  #nav {
    display: none;
  }
  .container {
    padding: 20px;
  }
  .max-width {
    margin-top: 0;
    padding: 25px;
  }
  .lorem {
    font-size: 15px;
  }
  .btn-to-chat {
    display: inline-block;
    position: relative;
    bottom: 0;
    margin-bottom: 5px;
  }
}
@media (max-width: 500px) {
  .container {
    padding: 10px;
  }
}
@import './assets/styles/Form.css';
</style>
