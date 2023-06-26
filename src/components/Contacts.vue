<script setup lang="ts">
import { ref, onMounted } from 'vue'

const props = defineProps<{
  token: string,
  tenant: string
}>()

const contacts = ref([])

onMounted(async () => {
  const contactsResponse = await fetch('https://my.1tool.com/suite/api/contacts', {
    headers: {
      accept: 'application/json',
      'X-Tenant': props.tenant,
      Authorization: `Bearer ${props.token}`
    }
  })
  contacts.value = (await contactsResponse.json()).data
})

const fields = ['firma', 'vorname', 'name', 'mail', 'handy', 'strasse', 'plz', 'ort']
</script>

<template class="root">
  <div class="root">
    <div class="row" v-for="contact in contacts">
      <div class="cell" v-for="field in fields">
        {{ contact[field] || '—' }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.root {
  display: flex;
  flex-direction: column;
}

.row {
  display: flex;
}

.cell {
  margin-left: 1rem;
  width: 7rem;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
</style>
