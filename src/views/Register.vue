<template>
  <div class="max-width">
    <h2 class="title title-register">Create an account</h2>
    <form @submit.prevent="processForm" class="form">
      <label for="email"
        :class="`form-label ${isInvalidInput ? 'invalid-input' : ''}`"
      >Email<span v-if="isInvalidInput">{{msgError}}</span></label>
      <input
        type="email"
        v-model="email"
        class="form-control"
        required
        autofocus
      >
      <label for="text"
        :class="`form-label ${isInvalidInput ? 'invalid-input' : ''}`"
      >Username<span v-if="isInvalidInput">{{msgError}}</span></label>
      <input
        type="text"
        v-model="username"
        class="form-control"
        required
      >
      <label for="password"
        :class="`form-label ${isInvalidInput ? 'invalid-input' : ''}`"
      >Password<span v-if="isInvalidInput">{{msgError}}</span></label>
      <input
        type="password"
        v-model="password"
        class="form-control"
        required
        minlength="6"
      >
      <button class="btn btn-primary">Register</button>
    </form>
    <router-link to="/login" class="link-account">Already have an account?</router-link>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import { createUserWithEmailAndPassword } from 'firebase/auth'
import { doc, setDoc } from 'firebase/firestore'
import { auth, db } from '../firebase'
import { useRouter } from 'vue-router'

export default {
  setup () {
    const isInvalidInput = ref(false)
    const msgError = ref('Input is invalid.')
    const email = ref('')
    const username = ref('')
    const password = ref('')
    const router = useRouter()

    const processForm = async () => {
      if (!email.value.trim() || !username.value.trim() || !password.value.trim()) {
        isInvalidInput.value = true
        msgError.value = 'Empty Entries'
        return
      }
      try {
        // REGISTER
        const userCreated = await createUserWithEmailAndPassword(auth, email.value, password.value)
        // Add a new document in collection "users"
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
        console.log(error.message)
        isInvalidInput.value = true
        msgError.value = error.message
      }
    }

    return { email, username, password, processForm, isInvalidInput, msgError }
  }
}
</script>
