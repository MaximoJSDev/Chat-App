<template>
  <div class="max-width">
    <h1 class="title-home">Welcome to Home
      {{userState ? userState.displayName || userState.email :'User'}}!
    </h1>
    <p class="lorem">
      Lorem ipsum, dolor sit amet consectetur adipisicing elit. Non, illo.
      Optio illum iure fuga qui ipsa itaque! Molestiae officia at minus sint corrupti dolorem.
      Eveniet nisi maxime culpa commodi dolor. Lorem ipsum dolor sit amet, consectetur adipisicing elit.
      Excepturi, quibusdam!
      Lorem ipsum, dolor sit amet consectetur adipisicing elit. Non, illo.
      Optio illum iure fuga qui ipsa itaque! Molestiae officia at minus sint corrupti dolorem.
      Eveniet nisi maxime culpa commodi dolor. Lorem ipsum dolor sit amet, consectetur adipisicing elit.
      Excepturi, quibusdam!
    </p>
    <router-link to="/chat" class="btn btn-to-chat">Go to Chats</router-link>
  </div>
</template>
<script>
import { inject } from 'vue'
export default {
  name: 'Home',
  setup () {
    const userState = inject('userState')

    return { userState }
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
  font-size: 18px;
  font-weight: 300;
  margin-bottom: 35px;
  max-width: 1120px;
}
.btn-to-chat {
  position: absolute;
  bottom: 40px;
}
</style>
