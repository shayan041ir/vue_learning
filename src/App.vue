
<script setup>
import { ref, computed } from 'vue'

const products = ['لپ‌تاپ', 'موس', 'کیبورد']
console.log(products[0]) // لپ‌تاپ
console.log(products[1]) // موس
console.log(products[2]) // کیبورد
console.log(products.length) // 3 


const search = ref('')

const filteredUsers = computed(() => {
  return users.value.filter(user =>
    user.name.toLowerCase().includes(search.value.toLowerCase())
  )
})

function addUser() {
  const newId = Math.max(0, ...users.value.map(user => user.id)) + 1

  users.value.push({
    id: newId,
    name: `User ${newId}`,
    age: 20
  })
}
</script>

<template>
  <main>
    <h1>پنل مدیریت کاربران</h1>

    <input
      v-model="search"
      placeholder="جست‌وجوی نام کاربر..."
    />

    <button @click="addUser">
      افزودن کاربر آزمایشی
    </button>

    <p>تعداد نتایج: {{ filteredUsers.length }}</p>

    <ul>
      <li
        v-for="user in filteredUsers"
        :key="user.id"
      >
        {{ user.name }} — {{ user.age }} سال
      </li>
    </ul>

    <p v-if="filteredUsers.length === 0">
      کاربری پیدا نشد.
    </p>
  </main>
</template>

<style scoped>
main {
  max-width: 600px;
  margin: 40px auto;
  padding: 24px;
  font-family: sans-serif;
}

input {
  padding: 10px;
  margin-right: 8px;
  margin-bottom: 12px;
}

button {
  padding: 10px;
  cursor: pointer;
}

li {
  margin: 12px 0;
}
</style>
