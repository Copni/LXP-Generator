<script setup>
import { ref, onMounted } from 'vue'

const userName = ref('')
const password = ref('')
const users = ref([])

const emits = defineEmits(['success', 'fail'])

// Charger users.json quand le composant est monté
onMounted(async () => {
  const res = await fetch('/users.json')
  users.value = await res.json()
})

// Vérifie login
function login() {
  const user = users.value.find(u => u.username === userName.value)
  if (user && user.password === password.value) {
    emits('success', userName.value)
    console.log('Login successful')
  } else {
    console.log('Login failed')
    emits('fail')
  }
}

// Enregistre un nouvel utilisateur (ajout au tableau local)
function register() {
  const exists = users.value.find(u => u.username === userName.value)
  if (exists) {
    emits('fail')
  } else {
    users.value.push({ username: userName.value, password: password.value })
    emits('success', userName.value)
  }
}
</script>

<template>
  <div id="logSection">
    <p>Username: {{ userName }}</p>
    <input v-model="userName" id="nameInput" type="text" required placeholder="Username" />
    <p>Password<br></p>
    <input v-model="password" id="pswInput" type="password" required placeholder="Password" />
    <div class="button-group">
      <button id="logInput" @click="login">Login</button>
      <button id="regInput" @click="register">Register</button>
    </div>
  </div>
</template>

<style scoped>
#logSection {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 200px;
  margin: 0 auto;
}

#logInput, #regInput {
  flex: 1;
}

.button-group {
  display: flex;
  gap: 10px;
}
</style>
