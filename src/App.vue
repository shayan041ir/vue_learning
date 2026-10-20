<script setup>
import { ref } from "vue";

const name = ref("");
const email = ref("");

const loading = ref(false);
const errorMsg = ref("");
const successMsg = ref("");
const createdUser = ref(null);

async function createUser() {
  errorMsg.value = "";
  successMsg.value = "";
  createdUser.value = null;

  if (!name.value.trim() || !email.value.trim()) {
    errorMsg.value = "لطفاً نام و ایمیل را وارد کن.";
    return;
  }

  loading.value = true;

  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        name: name.value.trim(),
        email: email.value.trim(),
      }),
    });

    if (!response.ok) {
      throw new Error("ثبت کاربر ناموفق بود.");
    }

    const data = await response.json();

    createdUser.value = data;
    successMsg.value = "کاربر با موفقیت ثبت شد!";

    name.value = "";
    email.value = "";
  } catch (error) {
    errorMsg.value = error.message || "خطایی رخ داد.";
  } finally {
    loading.value = false;
  }
}
</script>

<template>
  <main class="container">
    <h1>ثبت کاربر جدید</h1>

    <form @submit.prevent="createUser">
      <div class="form-group">
        <label for="name">نام کاربر</label>
        <input
          id="name"
          v-model="name"
          type="text"
          placeholder="مثلاً Shayan"
          :disabled="loading"
        />
      </div>

      <div class="form-group">
        <label for="email">ایمیل</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="مثلاً shayan@example.com"
          :disabled="loading"
        />
      </div>

      <button type="submit" :disabled="loading">
        {{ loading ? "در حال ثبت..." : "ثبت کاربر" }}
      </button>
    </form>

    <p v-if="errorMsg" class="error">
      {{ errorMsg }}
    </p>

    <p v-if="successMsg" class="success">
      {{ successMsg }}
    </p>

    <section v-if="createdUser" class="result">
      <h2>اطلاعات پاسخ سرور</h2>
      <p>شناسه: {{ createdUser.id }}</p>
      <p>نام: {{ createdUser.name }}</p>
      <p>ایمیل: {{ createdUser.email }}</p>
    </section>
  </main>
</template>








<style scoped>
.container {
  max-width: 500px;
  margin: 40px auto;
  padding: 24px;
  font-family: Arial, sans-serif;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 18px;
}

input {
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 16px;
}

button {
  width: 100%;
  padding: 12px;
  border: none;
  border-radius: 8px;
  background: #2563eb;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.error {
  color: #dc2626;
  margin-top: 16px;
}

.success {
  color: #15803d;
  margin-top: 16px;
}

.result {
  margin-top: 20px;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 10px;
  overflow-wrap: anywhere;
}
</style>
