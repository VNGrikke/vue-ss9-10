<template>
  <div>
    <input type="text" v-model="inputValue" />
    <p>gia tri: {{ inputValue }}</p>
  </div>
</template>

<script setup>
 import { ref } from "vue";
let inputValue = ref("");
</script>

<style>
</style>