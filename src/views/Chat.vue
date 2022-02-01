<template>
  <h1>Chat App</h1>
  <p>Messages...</p>
</template>

<script>
import { inject, watchEffect } from 'vue'
import { useRouter } from 'vue-router'
export default {
  setup () {
    const router = useRouter()
    const userState = inject('userState')
    console.log(userState.value)

    watchEffect(() => {
      if (userState.value === null || userState.value === undefined) {
        router.push('/login')
      }
    })

    return { userState }
  }
}
</script>
