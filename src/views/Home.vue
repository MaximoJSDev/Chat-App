<template>
  <h1 class="title-home">Welcome to Home
    {{userState ? userState.displayName || userState.email :'User'}}!
  </h1>
  <p>
    Lorem ipsum, dolor sit amet consectetur adipisicing elit. Non, illo.
    Optio illum iure fuga qui ipsa itaque! Molestiae officia at minus sint corrupti dolorem.
    Eveniet nisi maxime culpa commodi dolor. Lorem ipsum dolor sit amet, consectetur adipisicing elit.
    Excepturi, quibusdam!
  </p>
  <router-link to="/chat" class="btn btn-signup">Go to Chats</router-link>
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
p:not(.msg) {
  margin-bottom: 35px;
}
</style>
