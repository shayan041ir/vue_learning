<script setup>
import { ref } from "vue";

const name = ref("Shayan");
const count = ref(0);
const message = ref("به اولین پروژه Vue من خوش آمدی.");
const age = ref(23);
const user = ref({
  name: "Shayan",
  age: 23,
  email: "shayan@example.com"
});
const hobbies = ref(["خواندن", "نوشتن", "برنامه‌نویسی"]);

function increaseCount() {
  count.value++;
}

function decreaseCount() {
  if (count.value > 0) {
    count.value--;
  }
}
</script>

<template>
  <main>
    <h1>سلام {{ name }}!</h1>

    <p>{{ message }}</p>

    <p>تعداد کلیک‌ها: {{ count }}</p>

    <p>سن: {{ age }}</p>

    <p>اطلاعات کاربر:</p>
    <ul>
      <li>نام: {{ user.name }}</li>
      <li>سن: {{ user.age }}</li>
      <li>ایمیل: {{ user.email }}</li>
    </ul>

    <p>سرگرمی‌ها:</p>
    <ul>
      <li v-for="(hobby, index) in hobbies" :key="index">{{ hobby }}</li>
    </ul>

    <button @click="increaseCount">افزایش شمارنده</button>
    <button @click="count = 0">بازنشانی شمارنده</button>
    <button @click="decreaseCount">کاهش شمارنده</button>
  </main>
</template>

<style scoped>
main {
  background-color: #000000;
  max-width: 600px;
  margin: 80px auto;
  text-align: center;
  font-family: sans-serif;
}

button {
  padding: 10px 18px;
  cursor: pointer;
}
</style>
