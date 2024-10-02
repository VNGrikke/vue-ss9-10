<template>
  <div>
    <p>tien do: {{ inputValue4 }}%</p>
    <input type="range" v-model="inputValue4" />
  </div>
</template>

<script setup>
import { ref } from "vue";

let inputValue4 = ref("");
</script>

<style>
</style>
