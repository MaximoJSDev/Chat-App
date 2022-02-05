<template>
  <div class="overview">
    <section class="users-section z-index">
      <h2>Messages: </h2>
      <LoggedUsers />
    </section>
    <section class="contact-section">
      <ContactUser v-if="selectedUserID !== ''" />
      <SelectUser v-else />
    </section>
  </div>
</template>

<script>
import { inject, provide, ref, watchEffect } from 'vue'
import { useRouter } from 'vue-router'
import LoggedUsers from '../components/LoggedUsers.vue'
import ContactUser from '../components/ContactUser.vue'
import SelectUser from '../components/SelectUser.vue'

export default {
  components: { LoggedUsers, ContactUser, SelectUser },
  setup () {
    const router = useRouter()
    const myUser = inject('myUser')
    const selectedUserID = ref('')
    provide('selectedUserID', selectedUserID)

    // Si el usuario no esta registrado envialo a /login
    watchEffect(() => {
      if (myUser.value === null || myUser.value === undefined) router.push('/login')
    })

    return { selectedUserID }
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
  overflow: hidden;
}
body.dark-mode .overview section {
  background-color: #1f2633;
}
.overview h2 {
  margin-bottom: 15px;
}
.users-section {
  min-width: 390px;
}
@media (max-width: 1200px) {
  .overview section {
    padding: 22px;
  }
  .users-section {
    min-width: 340px;
  }
}
@media (max-width: 1020px) {
  .overview {
    position: relative;
  }
  .overview section {
    position: absolute;
    width: 100%;
  }
  .users-section.z-index {
    z-index: 1;
  }
  .contact-section h2, .messages-container {
    padding: 0 18px !important;
  }
  .form-chat {
    padding: 0 18px 18px;
  }
}
</style>
