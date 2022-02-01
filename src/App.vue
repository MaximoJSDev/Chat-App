<template>
  <TheNavbar :userState="userState" :logout="logout" />
  <main class="container">
    <router-view />
  </main>
</template>

<script>
import { ref, provide } from 'vue'
import { onAuthStateChanged, signOut } from '@firebase/auth'
import { auth, db } from './firebase'
import TheNavbar from './components/TheNavbar.vue'
import { doc, updateDoc } from '@firebase/firestore'
export default {
  components: { TheNavbar },
  setup () {
    const userState = ref(auth.currentUser)

    // Si el estado del usuario cambia...
    onAuthStateChanged(auth, (user) => {
      userState.value = user
    })
    provide('userState', userState)

    const logout = async () => {
      try {
        await updateDoc(doc(db, 'users', userState.value.uid), {
          state: false
        })
        await signOut(auth)
      } catch (error) {
        console.log(error)
      }
    }

    return { userState, logout }
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
  background-repeat: no-repeat;
  font-family: 'Poppins', sans-serif;
  /* background-color: #1a1e25;
  color: #fff; */
}
#app {
  height: 100vh;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.container {
  padding: 0 40px;
  width: 1040px;
  margin: auto;
  min-height: calc(100vh - 64px);
  height: calc(100vh - 64px);
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
</style>
