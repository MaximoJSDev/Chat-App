<template>
  <div class="overview">
    <section class="users-section">
      <h2>Messages: </h2>
      <Users />
    </section>
    <section class="contact-section">
      <div v-if="selectedUserID !== ''" ref="RefChat">
        <h2>Chat: </h2>
        <div class="messages-container">
          <div
            v-for="message in allMessages"
            :key="message.id"
            :class="message.uid === myUser.uid ? 'message my-message' : 'message'"
          >
            <h3>{{message.user}}</h3>
            <p>{{message.texto}}</p>
          </div>
        </div>
        <form class="form-chat" @submit.prevent="sendMessage">
          <input type="text" placeholder="Write a message..." v-model="messageForm">
          <button class="form-chat__send-btn"><img src="../assets/Send.svg" alt="Send icon"></button>
        </form>
      </div>
      <div class="selectUser" v-else>
        <h5>
          Select a user to start a conversation ✌️🤖
        </h5>
      </div>
    </section>
  </div>
</template>

<script>
import { inject, provide, ref, watch, watchEffect } from 'vue'
import { useRouter } from 'vue-router'
import Users from '../components/Users.vue'
import { db, timeStamp } from '../firebase'
import { addDoc, collection, doc, onSnapshot, query } from 'firebase/firestore'

export default {
  components: { Users },
  setup () {
    const router = useRouter()
    const myUser = inject('myUser')
    const myUserNAME = ref('')
    const selectedUserID = ref('')
    const allMessages = ref([])
    const messageForm = ref('')
    const RefChat = ref(null)
    let unsuscribe

    provide('selectedUserID', selectedUserID)
    provide('myUserNAME', myUserNAME)

    watchEffect(() => {
      if (myUser.value === null || myUser.value === undefined) {
        router.push('/login')
      }
    })

    const getMessages = (selectedUserID) => {
      allMessages.value = []
      const q = query(collection(db, 'chat', myUser.value.uid, selectedUserID))
      unsuscribe = onSnapshot(q, (snapshot) => {
        snapshot.docChanges().forEach((change) => {
          if (change.type === 'added') {
            allMessages.value.push({ ...change.doc.data(), id: change.doc.id })
          }
          if (RefChat.value !== null) {
            setTimeout(() => {
              window.scrollTo(0, RefChat.value.scrollHeight)
            }, 100)
          }
        })
      })
    }

    watch(
      () => selectedUserID.value,
      (newUID) => {
        if (unsuscribe) {
          unsuscribe()
          if (newUID) {
            getMessages(newUID)
          }
        } else {
          getMessages(newUID)
        }
      }
    )

    const sendMessage = async () => {
      if (!messageForm.value.trim()) return
      try {
        const messageToSend = {
          user: myUserNAME.value[0].username,
          texto: messageForm.value,
          date: timeStamp(),
          uid: myUser.value.uid
        }

        const docRef = doc(db, 'chat', myUser.value.uid)
        const colRef = collection(docRef, selectedUserID.value)
        addDoc(colRef, messageToSend)

        const docRef2 = doc(db, 'chat', selectedUserID.value)
        const colRef2 = collection(docRef2, myUser.value.uid)
        addDoc(colRef2, messageToSend)

        messageForm.value = ''
      } catch (error) {
        console.log(error)
      }
    }

    return {
      RefChat,
      myUser,
      selectedUserID,
      getMessages,
      allMessages,
      sendMessage,
      messageForm
    }
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
.overview h2 {
  margin-bottom: 15px;
}
.users-section {
  min-width: 390px;
}

.contact-section {
  flex-grow: 6;
}
.messages-container {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  row-gap: 7px;
}
.message {
  min-width: 46%;
  font-size: 15px;
  line-height: 24px;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  flex-direction: column;
  padding: 7px 20px;
  box-shadow: 0px 13px 35px #1a1e25;
  border-radius: 22px;
  margin-right: auto;
}
.my-message {
  align-items: flex-end;
  background-color: #19997e;
  margin-left: auto;
  margin-right: unset;
  text-shadow: 2px 2px 6px #1f26339e;
}
.message:not(.my-message) {
  background-color: #252d3c;
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
  padding: 12px;
  margin-left: 14px;
  background-color: #3aa58b;
  cursor: pointer;
}
.form-chat img {
  height: 36px;
}

.selectUser {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.selectUser h5 {
  font-size: 35px;
  text-align: center;
}
</style>
