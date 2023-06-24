<script setup lang="ts">
import { ref } from 'vue'
import Contacts from './components/Contacts.vue'

const token = ref('')
const tenant = ref('')

const email = ref('')
const password = ref('')

async function login() {
  const authData = await fetch('https://my.1tool.com/suite/api/auth/user', {
    method: 'POST',
    headers: {
      accept: 'application/json',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      email: email.value,
      password: password.value
    })
  })
  const authDataJson = await authData.json()

  token.value = authDataJson.api_token
  tenant.value = authDataJson.tenant_identifier
}
</script>

<template>
  <Contacts v-if="token && tenant" :token="token" :tenant="tenant" />
  <div class="login" v-else>
    Email:
    <input v-model="email" type="email" />
    Password:
    <input v-model="password" type="password" />
    <button class="button" @click="login">Login</button>
  </div>
</template>

<style scoped>
.login {
  display: flex;
  flex-direction: column;
}

.button {
  margin-top: 0.5rem;
}
</style>
