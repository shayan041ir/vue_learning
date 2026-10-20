
<script setup>
import { ref } from 'vue'

const apiusers = ref([])
const loading = ref(true)
const errorMsg = ref('')

async function getUsers() {
  loading.value = true
  errorMsg.value = ''

  try {
    const response = await fetch(
      'https://jsonplaceholder.typicode.com/users'
    )

    if (!response.ok) {
      throw new Error('دریافت اطلاعات ناموفق بود.')
    }

    const data = await response.json()

    apiusers.value = data
  } catch (error) {
    errorMsg.value = error.message
  } finally {
    loading.value = false
  }
}

getUsers()
</script>

<template>
  <main>
    <h1>فهرست کاربران</h1>

    <p v-if="loading">
      در حال دریافت اطلاعات...
    </p>

    <div v-else-if="errorMsg">
      <p>{{ errorMsg }}</p>
      <button @click="getUsers">تلاش مجدد</button>
    </div>

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
  margin-bottom: 12px;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 10px;
}

.user-card p {
  overflow-wrap: anywhere;
}
</style>
