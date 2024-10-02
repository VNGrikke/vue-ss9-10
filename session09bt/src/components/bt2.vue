<template>
  <div>
    <input type="color" v-model="inputValue2" />
    <p>gia tri: {{ inputValue2 }}</p>
  </div>
</template>

<script setup>
 import { ref } from "vue";
let inputValue2 = ref("");
</script>

<style>
</style>