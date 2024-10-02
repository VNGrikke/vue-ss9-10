<template>
  <div>
    <p>Ngay sinh: {{ inputValue3 }}</p>
    <input type="date" v-model="inputValue3" />
  </div>
</template>

<script setup>
import { ref } from "vue";

let inputValue3 = ref("");
</script>

<style>
</style>
