<script setup>
import { ref } from "vue";

const name = ref("Shayan");
const count = ref(0);
const message = ref("به اولین پروژه Vue من خوش آمدی.");
const age = ref(23);

function increaseCount() {
  count.value++;
}

function decreaseCount() {
  count.value--;
}
</script>

<template>
  <main>
    <h1>سلام {{ name }}!</h1>

    <p>{{ message }}</p>

    <p>تعداد کلیک‌ها: {{ count }}</p>

    <p>سن: {{ age }}</p>

    <button @click="increaseCount">افزایش شمارنده</button>
    <button @click="count = 0">بازنشانی شمارنده</button>
    <button @click="decreaseCount">کاهش شمارنده</button>
  </main>
</template>

<style scoped>
main {
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
