<script setup>
import { reactive, onMounted } from 'vue'

const url = 'https://lab5-p379.onrender.com/api/v1/messages/'

const emit = defineEmits(['close'])

const messages = reactive([])

const form = reactive({
  user: '',
  text: ''
})

onMounted(async () => {
  const res = await fetch(url)
  const data = await res.json()

  // de nieuwste berichten staan achteraan, dus omdraaien
  data.reverse().forEach((message) => {
    messages.push(message)
  })
})

async function sendMessage() {
  if (!form.user.trim() || !form.text.trim()) {
    return
  }

  const res = await fetch(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ user: form.user.trim(), text: form.text.trim() })
  })
  const newMessage = await res.json()

  // bovenaan toevoegen, de lijst past zich meteen aan
  messages.unshift(newMessage)
  form.text = ''
}
</script>

<template>
  <div class="chat">
    <div class="chat-top">
      <h4>Reacties ({{ messages.length }})</h4>
      <button @click="emit('close')">Sluiten</button>
    </div>

    <ul>
      <li v-for="message in messages" :key="message._id">
        <span class="user">{{ message.user }}</span>
        <span class="text">{{ message.text }}</span>
      </li>
    </ul>

    <form @submit.prevent="sendMessage">
      <input v-model="form.user" placeholder="Jouw naam" />
      <input v-model="form.text" placeholder="Schrijf een reactie..." />
      <button type="submit">Verstuur</button>
    </form>
  </div>
</template>

<style scoped>
.chat {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60vh;
  overflow-y: auto;
  background: #fff;
  color: #000;
}
</style>
