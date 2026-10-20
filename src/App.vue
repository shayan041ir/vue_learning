
<script setup>
import { ref } from 'vue'

const apiusers = ref([])
const loading = ref(true)
const errorMsg = ref('')

function getUsers() {
  loading.value = true
  errorMsg.value = ''

  fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => {
      if (!response.ok) {
        throw new Error('دریافت اطلاعات از سرور ناموفق بود.')
      }

      return response.json()
    })
    .then(data => {
      apiusers.value = data
    })
    .catch(error => {
      errorMsg.value = error.message
    })
    .finally(() => {
      loading.value = false
    })
}

getUsers()
</script>

<template>
  <main>
    <h1>فهرست کاربران</h1>

    <p v-if="loading">
      در حال دریافت اطلاعات...
    </p>

    <p v-else-if="errorMsg">
      {{ errorMsg }}
    </p>

    <div v-else>
      <article
        v-for="user in apiusers"
        :key="user.id"
        class="user-card"
      >
        <h3>{{ user.name }}</h3>
        <p>شناسه: {{ user.id }}</p>
        <p>ایمیل: {{ user.email }}</p>
      </article>

      <p v-if="apiusers.length === 0">
        کاربری برای نمایش وجود ندارد.
      </p>
    </div>
  </main>
</template>

<style scoped>
main {
  max-width: 700px;
  margin: 40px auto;
  padding: 16px;
  font-family: Arial, sans-serif;
}

.user-card {
  padding: 16px;
  margin-bottom: 12px;
  border: 1px solid #ddd;
  border-radius: 10px;
}

.user-card p {
  overflow-wrap: anywhere;
}
</style>
