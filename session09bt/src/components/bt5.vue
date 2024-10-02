<template>
  <div>
    <p>So thich: {{ inputValue5 }}</p>
    <ul>
        <li><input type="radio" v-model="inputValue5" value="nam">nam</li>
        <li><input type="radio" v-model="inputValue5" value="nu">nu</li>
        <li><input type="radio" v-model="inputValue5" value="bi mat">bi mat</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

let inputValue5 = ref("");
</script>

<style>
</style>
