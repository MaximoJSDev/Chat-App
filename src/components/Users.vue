<template>
  <div
    v-for="user in users"
    :key="user.uid"
    :class="user.state ? 'connected-user user' : 'offline-user user'"
  >
  <div class="btn-nav profile">
    <img src="../assets/Profile.svg" alt="Image user">
  </div>
    <div>
      <h6 class="user__title">{{user.username || user.email}}</h6>
      <p class="user__msg">Why not? Say hello!</p>
    </div>
      <span class="user__time">09:00</span>
  </div>
</template>

<script>
import { collection, onSnapshot, query } from 'firebase/firestore'
import { db } from '../firebase'
import { ref } from 'vue'
export default {
  setup () {
    const users = ref([])
    const q = query(collection(db, 'users'))

    onSnapshot(q, (snapshot) => {
      snapshot.docChanges().forEach((change) => {
        if (change.type === 'added') {
          console.log('New user: ', change.doc.data())
          users.value = [...users.value, change.doc.data()]
          users.value = users.value.sort((a, b) => b.state - a.state)
        }
        if (change.type === 'modified') {
          console.log('Modified user: ', change.doc.data())
          users.value = users.value.map(user =>
            user.uid === change.doc.data().uid
              ? { ...user, state: change.doc.data().state }
              : user
          )
          users.value = users.value.sort((a, b) => b.state - a.state)
        }
        if (change.type === 'removed') {
          console.log('Removed user: ', change.doc.data())
        }
      })
    })

    return { users }
  }
}
</script>

<style>
.offline-user {
  color: #b0bacc;
}
.user {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 10px 0;
  border-bottom: 1px solid #e1e1e1c7;
}
body.dark-mode .user {
  border-bottom-color: #e5e5e51f;
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
