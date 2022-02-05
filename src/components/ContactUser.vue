<template>
  <div ref="RefChat">
    <header class="contact-section__header">
      <img src="../assets/ArrowLeft.svg" alt="arrow left icon" @click="backToListUsers">
      <h2>Chat: </h2>
    </header>
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
</template>

<script>
import { ref, inject, watchEffect } from 'vue'
import { addDoc, collection, doc, onSnapshot, orderBy, query } from '@firebase/firestore'
import { db, timeStamp } from '../firebase'

export default {
  setup () {
    const RefChat = ref(null)
    const allMessages = ref([])
    const messageForm = ref('')
    const myUserNAME = inject('myUserNAME')
    const myUser = inject('myUser')
    const selectedUserID = inject('selectedUserID')
    let unsuscribe

    // Si envias un mensaje: (lo envia a la base de datos)
    const sendMessage = async () => {
      if (!messageForm.value.trim()) return
      try {
        const messageToSend = {
          user: myUserNAME.value,
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

    // Obten los mensajes de la base de datos
    const getMessages = (selectedUserID) => {
      allMessages.value = []
      // Ordena los mensajes por la fechat
      const q = query(collection(db, 'chat', myUser.value.uid, selectedUserID), orderBy('date'))
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

    // Si el ID del usuario seleccionado cambia:
    watchEffect(() => {
      if (unsuscribe) {
        unsuscribe()
        if (selectedUserID.value) {
          getMessages(selectedUserID.value)
        }
      } else {
        getMessages(selectedUserID.value)
      }
    })

    const backToListUsers = () => {
      document.querySelector('.users-section').classList.add('z-index')
    }

    return { allMessages, RefChat, messageForm, sendMessage, myUser, backToListUsers }
  }
}
</script>
<style>
.contact-section.contact-section {
  flex-grow: 6;
  padding-left: 0;
  padding-right: 0;
}
.contact-section__header {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding-left: 24px;
  margin-bottom: 15px;
}
.contact-section__header img {
  padding: 4px;
  border-radius: 50%;
  background-color: #fff;
}
.contact-section__header h2 {
  margin-bottom: 0 !important;
  padding-left: 17px;
}
.contact-section > div {
  height: calc(100% - 60px);
}
.messages-container {
  display: flex;
  flex-direction: column;
  row-gap: 7px;
  height: inherit;
  overflow-y: scroll;
  padding: 0 30px;
}
.messages-container::-webkit-scrollbar {
  width: 6px;
  background-color: #252d3c;
}
.messages-container::-webkit-scrollbar-thumb {
  background-color: #2f3944;
  border-radius: 10px;
}
body:not(.dark-mode) .messages-container::-webkit-scrollbar {
  background-color: #1f263363 !important;
}
body:not(.dark-mode) .messages-container::-webkit-scrollbar-thumb {
  background-color: #1f263369 !important;
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
body:not(.dark-mode) .message {
  box-shadow: 2px 10px 24px #1a1e2525;
  color: #fff;
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
  bottom: 0;
  padding: 0 30px 30px;
}
body.dark-mode .form-chat {
  background-color: #1f2633;
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
</style>
