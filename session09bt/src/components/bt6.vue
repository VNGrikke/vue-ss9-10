<template>
  <div>
    <p>so thich: {{ inputValue6.join(", ") }}</p>
    <ul>
      <li>
        <input type="checkbox" v-model="inputValue6" id="1" value="choi" />Chơi
      </li>
      <li>
        <input type="checkbox" v-model="inputValue6" id="2" value="ngu" />Ngủ
      </li>
      <li>
        <input type="checkbox" v-model="inputValue6" id="3" value="code" />Code
      </li>
      <li>
        <input type="checkbox" v-model="inputValue6" id="4" value="an" />Ăn
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

let inputValue6 = ref([]);
</script>

<style>
</style>
