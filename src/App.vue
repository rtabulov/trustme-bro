<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PhoneNumberInput from "./components/PhoneNumberInput.vue";

const phone = ref("+77471234567");
watchEffect(() => {
  console.log({ phone: phone.value });
});
</script>

<template>
  <div class="bg-slate-900 min-h-screen text-slate-200 py-12">
    <main class="py-12 px-12 container mx-auto bg-slate-100 text-slate-900">
      <h1 class="text-3xl mb-4">Trustme test task</h1>
      <p><a href="https://github.com/rtabulov/trustme-bro"></a></p>
      <PhoneNumberInput v-model="phone" />
    </main>
  </div>
</template>
