<template>
  <div class="overview">
    <section class="users-section">
      <h1>Messages: </h1>
      <Users />
    </section>
    <section class="contact-section">
      <h1>Hello There! </h1>
      <form class="form-chat">
        <input type="text" placeholder="Write a message...">
        <button class="form-chat__send-btn"><img src="../assets/Send.svg" alt="Send icon"></button>
      </form>
    </section>
  </div>
</template>

<script>
import { inject, watchEffect } from 'vue'
import { useRouter } from 'vue-router'
import Users from '../components/Users.vue'
export default {
  components: { Users },
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
  column-gap: 32px;
}
.overview section {
  position: relative;
  padding: 30px;
  border-radius: 30px;
  height: 100%;
  background-color: #fff;
}
body.dark-mode .overview section {
  background-color: #1f2633;
}
.users-section {
  flex-grow: 2;
  max-width: 460px;
}

.contact-section {
  flex-grow: 6;
}
.form-chat {
  width: 100%;
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: space-between;
  right: 0;
  bottom: 30px;
  padding: 0 30px;
}
.form-chat input {
  width: 94%;
  padding: 20px 20px;
  border-radius: 27px;
  border: none;
  outline: none;
  background-color: #ebecf2;
  font-size: 16px;
}
.form-chat button {
  display: flex;
  border: none;
  outline: none;
  border-radius: 50%;
  padding: 10px;
  margin-left: 14px;
  background-color: #3aa58b;
}
.form-chat img {
  height: 36px;
}
</style>
