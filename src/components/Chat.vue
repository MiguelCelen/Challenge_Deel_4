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
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 460px;
  height: 62vh;
  display: flex;
  flex-direction: column;
  background: #fff;
  color: #161823;
  border-radius: 14px 14px 0 0;
}

.chat-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 16px;
  border-bottom: 1px solid #ececec;
}

.chat-top h4 {
  font-size: 0.95rem;
}

.chat-top button {
  background: none;
  border: none;
  color: #8a8b91;
  cursor: pointer;
}

ul {
  flex: 1;
  overflow-y: auto;
  list-style: none;
  padding: 6px 16px;
}

li {
  display: flex;
  flex-direction: column;
  padding: 10px 0;
  border-bottom: 1px solid #f4f4f4;
}

.user {
  font-size: 0.8rem;
  font-weight: 600;
  color: #8a8b91;
}

.text {
  font-size: 0.95rem;
  margin-top: 2px;
  overflow-wrap: anywhere;
}

form {
  display: flex;
  gap: 6px;
  padding: 10px 12px;
  border-top: 1px solid #ececec;
}

input {
  min-width: 0;
  padding: 10px;
  border: none;
  border-radius: 8px;
  background: #f1f1f2;
}

input:first-child {
  width: 32%;
}

input:nth-child(2) {
  flex: 1;
}

form button {
  padding: 0 12px;
  border: none;
  border-radius: 8px;
  background: #fe2c55;
  color: #fff;
  font-weight: 600;
  cursor: pointer;
}
</style>
