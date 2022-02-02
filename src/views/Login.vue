<template>
  <div class="max-width">
    <h1 class="title">Welcome back!</h1>
    <p class="msg">We're so excited to see you again!</p>
    <form @submit.prevent="processForm" class="form">
      <label for="email" class="form-label">Email: </label>
      <input type="email" v-model="email" class="form-control">
      <label for="password" class="form-label">Password: </label>
      <input type="password" v-model="password" class="form-control">
      <button class="btn btn-primary">Login</button>
    </form>
    <router-link to="/register" class="link-account">Need an account? <span>Register</span></router-link>
  </div>
</template>

<script>
import { ref, inject } from 'vue'
import { useRouter } from 'vue-router'
import { signInWithEmailAndPassword } from 'firebase/auth'
import { doc, updateDoc } from 'firebase/firestore'
import { auth, db } from '../firebase'

export default {
  setup () {
    const email = ref('')
    const password = ref('')
    const userState = inject('userState')
    const router = useRouter()

    const processForm = async () => {
      if (!email.value.trim() && !password.value.trim()) return
      try {
        // LOGIN
        await signInWithEmailAndPassword(auth, email.value, password.value)

        await updateDoc(doc(db, 'users', userState.value.uid), {
          state: true
        })
        email.value = ''
        password.value = ''
        router.push('/chat')
      } catch (error) {
        console.log(error)
      }
    }

    return { email, password, processForm, userState }
  }
}
</script>
