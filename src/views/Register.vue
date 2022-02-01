<template>
  <h1 class="title">Create an account</h1>
  <form @submit.prevent="processForm" class="form">
    <label for="email" class="form-label">Email: </label>
    <input type="email" v-model="email" class="form-control">
    <label for="text" class="form-label">Username: </label>
    <input type="text" v-model="username" class="form-control">
    <label for="password" class="form-label">Password: </label>
    <input type="password" v-model="password" class="form-control">
    <button class="btn btn-primary">Register</button>
  </form>
  <router-link to="/login" class="link-account">Already have an account?</router-link>
</template>

<script>
import { ref } from '@vue/reactivity'
import { createUserWithEmailAndPassword } from 'firebase/auth'
import { doc, setDoc } from 'firebase/firestore'
import { auth, db } from '../firebase'
import { useRouter } from 'vue-router'

export default {
  setup () {
    const email = ref('')
    const username = ref('')
    const password = ref('')
    const router = useRouter()

    const processForm = async () => {
      if (!email.value.trim() && !password.value.trim()) return
      try {
        // REGISTER
        const userCreated = await createUserWithEmailAndPassword(auth, email.value, password.value)
        // Add a new document in collection "cities"
        await setDoc(doc(db, 'users', userCreated.user.uid), {
          username: username.value,
          email: userCreated.user.email,
          state: true,
          uid: userCreated.user.uid
        })

        email.value = ''
        password.value = ''
        router.push('/chat')
      } catch (error) {
        console.log(error)
      }
    }

    return { email, username, password, processForm }
  }
}
</script>
<style>
  @import '../assets/styles/Form.css';
</style>
