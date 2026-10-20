
<script setup>
import { ref, computed } from 'vue'

const users = ref([
  {
    id: 1,
    name: 'Shayan',
    age: 21,
    email: 'shayan@example.com'
  },
  {
    id: 2,
    name: 'Ali',
    age: 24,
    email: 'ali@example.com'
  }
])

const form = ref({
  name: '',
  age: '',
  email: ''
})

const search = ref('')
const message = ref('')

const filteredUsers = computed(() => {
  const keyword = search.value.trim().toLowerCase()

  return users.value.filter(user =>
    user.name.toLowerCase().includes(keyword) ||
    user.email.toLowerCase().includes(keyword)
  )
})

function addUser() {
  const name = form.value.name.trim()
  const email = form.value.email.trim()
  const age = Number(form.value.age)

  if (!name || !email || !form.value.age) {
    message.value = 'لطفاً تمام فیلدها را تکمیل کن.'
    return
  }

  if (!Number.isFinite(age) || age < 1 || age > 120) {
    message.value = 'سن باید بین ۱ تا ۱۲۰ باشد.'
    return
  }

  const newUser = {
    id: Date.now(),
    name,
    age,
    email
  }

  users.value.push(newUser)

  form.value = {
    name: '',
    age: '',
    email: ''
  }

  message.value = 'کاربر با موفقیت اضافه شد.'
}

function deleteUser(id) {
  users.value = users.value.filter(user => user.id !== id)
  message.value = 'کاربر حذف شد.'
}
</script>

<template>
  <main class="container">
    <h1>پنل مدیریت کاربران</h1>

    <section class="card">
      <h2>افزودن کاربر</h2>

      <!-- .prevent از رفتار پیش‌فرض مرورگر، یعنی بارگذاری مجدد صفحه، جلوگیری می‌کند. -->
      <form @submit.prevent="addUser">
        <label>
          نام:
          <input v-model="form.name" type="text" placeholder="نام کاربر">
        </label>

        <label>
          سن:
          <input v-model="form.age" type="number" min="1" max="120" placeholder="سن">
        </label>

        <label>
          ایمیل:
          <input v-model="form.email" type="email" placeholder="example@email.com">
        </label>

        <button type="submit">افزودن کاربر</button>
      </form>

      <p v-if="message" class="message">{{ message }}</p>
    </section>

    <section class="card">
      <h2>فهرست کاربران ({{ users.length }})</h2>

      <input
        v-model="search"
        type="search"
        placeholder="جست‌وجوی نام یا ایمیل..."
      >

      <p v-if="filteredUsers.length === 0">
        کاربری پیدا نشد.
      </p>

      <article
        v-for="user in filteredUsers"
        :key="user.id"
        class="user"
      >
        <div>
          <h3>{{ user.name }}</h3>
          <p>سن: {{ user.age }}</p>
          <p>ایمیل: {{ user.email }}</p>
        </div>

        <button
          type="button"
          class="delete"
          @click="deleteUser(user.id)"
        >
          حذف
        </button>
      </article>
    </section>
  </main>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f1f5f9;
  color: #1e293b;
}

.container {
  max-width: 760px;
  margin: 35px auto;
  padding: 16px;
}

.card {
  background: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 12px;
}

form {
  display: grid;
  gap: 12px;
}

label {
  display: grid;
  gap: 6px;
}

input {
  width: 100%;
  padding: 11px;
  border: 1px solid #cbd5e1;
  border-radius: 7px;
}

button {
  padding: 10px 15px;
  border: 0;
  border-radius: 7px;
  cursor: pointer;
  background: #2563eb;
  color: white;
}

.user {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 12px 0;
  border-bottom: 1px solid #e2e8f0;
}

.user p {
  overflow-wrap: anywhere;
}

.delete {
  background: #dc2626;
}

.message {
  margin-bottom: 0;
  color: #2563eb;
}
</style>
