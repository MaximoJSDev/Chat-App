<template>
  <div
    v-for="user in getRegisteredUsersWithoutMe"
    :key="user.uid"
    :class="user.state ? 'connected-user user' : 'offline-user user'"
    :data-uid="user.uid"
    @click="getChatUser(user.uid)"
  >
  <div class="btn-nav profile" >
    <img src="../assets/Profile.svg" alt="User icon">
  </div>
    <div>
      <h6 class="user__title">{{user.username}}</h6>
      <p class="user__msg">Why not? Say hello!</p>
    </div>
      <span class="user__time">09:00</span>
  </div>
</template>

<script>
import { computed, inject, ref } from 'vue'
import { collection, onSnapshot, query } from 'firebase/firestore'
import { db } from '../firebase'

export default {
  setup () {
    const registeredUsers = ref([])
    const myUserNAME = inject('myUserNAME')
    const selectedUserID = inject('selectedUserID')
    const myUser = inject('myUser')

    // Obtiene todos los usarios registrados
    const q = query(collection(db, 'users'))
    onSnapshot(q, (snapshot) => {
      snapshot.docChanges().forEach((change) => {
        if (change.type === 'added') {
          registeredUsers.value = [...registeredUsers.value, change.doc.data()]
          registeredUsers.value = registeredUsers.value.sort((a, b) => b.state - a.state)
        }
        if (change.type === 'modified') {
          registeredUsers.value = registeredUsers.value.map(user =>
            user.uid === change.doc.data().uid
              ? { ...user, state: change.doc.data().state }
              : user
          )
          registeredUsers.value = registeredUsers.value.sort((a, b) => b.state - a.state)
        }
      })
    })

    // Quita al propio usuario del array de todos los usuarios
    const getRegisteredUsersWithoutMe = computed(() => {
      return registeredUsers.value.filter((item) => item.uid !== myUser.value.uid)
    })

    // Al dar click a un usuario:
    const getChatUser = (uid) => {
      document.querySelectorAll('.user').forEach((element) => {
        element.dataset.uid === uid
          ? element.classList.add('selectedUser')
          : element.classList.remove('selectedUser')
      })
      // Obtiene el nombre de tu usuario y lo guarda
      myUserNAME.value = registeredUsers.value.filter((item) => item.uid === myUser.value.uid)
      myUserNAME.value = myUserNAME.value[0].username
      selectedUserID.value = uid

      // Cambiar de section (responsive < 1020)
      document.querySelector('.users-section').classList.remove('z-index')
    }

    return { getRegisteredUsersWithoutMe, getChatUser }
  }
}
</script>

<style>
body.dark-mode .offline-user {
  color: #3b3e54;
}
.offline-user {
  color: #b0bacc;
}
.user {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 10px;
  border-radius: 13px;
  cursor: pointer;
  margin-bottom: 10px;
  transition: 0.3s;
}
.user:hover {
  transform: translateY(-5px);
  background-color: #293142;
}
.selectedUser {
  background-color: #293142;
}
.user .profile img {
  box-shadow: none;
  margin-right: 14px;
  background-image: linear-gradient(60deg, #29323c 0%, #485563 100%);
}
.user__title {
  font-size: 18px;
}
.user__time {
  text-align: end;
  flex-grow: 2;
}
</style>
