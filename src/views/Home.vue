<template>
  <div class="max-width">
    <h1 class="title-home">Welcome to Home
      {{myUser ? myUser.displayName || myUser.email :'User'}}!
    </h1>
    <p class="lorem">
      This is a real-time chat application, in which
      you can send messages to other registered users,
      before that, you must register and create a new account,
      or if you already have an account, just LogIn. 👨‍💻
    </p>
    <router-link to="/chat" class="btn btn-to-chat">Go to Chats</router-link>
  </div>
</template>
<script>
import { inject } from 'vue'
export default {
  name: 'Home',
  setup () {
    const myUser = inject('myUser')

    return { myUser }
  }
}
</script>
<style>
.title-home {
  font-size: 30px;
  line-height: 37px;
  margin-bottom: 10px;
}
.lorem {
  font-size: 20px;
  font-weight: 300;
  margin-bottom: 35px;
  max-width: 1120px;
}
.btn-to-chat {
  position: absolute;
  bottom: 40px;
}
</style>
