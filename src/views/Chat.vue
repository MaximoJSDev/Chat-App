<template>
  <div class="overview">
    <section class="users-section">
      <h1>Users: </h1>
      <TheChat />
    </section>
    <section class="contact-section">
      <h1>Hello There! </h1>
      <form>
        <input type="text" placeholder="Write a message...">
        <button>Send</button>
      </form>
    </section>
  </div>
</template>

<script>
import { inject, watchEffect } from 'vue'
import { useRouter } from 'vue-router'
import TheChat from '../components/TheChat.vue'
export default {
  components: { TheChat },
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
<style>
.overview {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  height: 100%;
}
.users-section {
  padding: 50px 50px 0 10px;
  /* border: 1px solid gray; */
  border-right: 2px solid #cbcbcb;
  height: 100%;
  flex-grow: 2;
}
.contact-section {
  padding: 50px 10px 0 50px;
  flex-grow: 6;
  height: 100%;
}
</style>
