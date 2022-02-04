<template>
  <div class="max-width">
    <h2 class="title">Welcome back!</h2>
    <p class="msg">We're so excited to see you again!</p>
    <form @submit.prevent="processForm" class="form">
      <label for="email" :class="`form-label ${isInvalidInput ? 'invalid-input' : ''}`">
        Email<span v-if="isInvalidInput">Login or password is invalid.</span>
      </label>
      <input type="email" v-model="email" class="form-control" required autofocus>
      <label for="password" :class="`form-label ${isInvalidInput ? 'invalid-input' : ''}`">
        Password<span v-if="isInvalidInput">Login or password is invalid.</span>
      </label>
      <input type="password" v-model="password" class="form-control" required minlength="6">
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
    const router = useRouter()
    const isInvalidInput = ref(false)
    const email = ref('')
    const password = ref('')
    const myUser = inject('myUser')

    const processForm = async () => {
      isInvalidInput.value = false
      if (!email.value.trim() || !password.value.trim()) {
        isInvalidInput.value = true
        return
      }
      try {
        // LOGIN
        await signInWithEmailAndPassword(auth, email.value, password.value)

        await updateDoc(doc(db, 'users', myUser.value.uid), {
          state: true
        })
        email.value = ''
        password.value = ''
        router.push('/chat')
      } catch (error) {
        console.log(error.message)
        isInvalidInput.value = true
      }
    }

    return { email, password, processForm, isInvalidInput, myUser }
  }
}
</script>
