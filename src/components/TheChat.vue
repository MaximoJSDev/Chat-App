<template>
    <div
      v-for="user in users"
      :key="user.uid"
      :class="user.state ? 'connected-user' : 'offline-user'"
    >
      <h4>{{user.username || user.email}}</h4>
      <p>Why not? Say hello!</p>
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
</style>
