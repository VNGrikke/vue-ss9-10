<template>
  <div>
    <form @submit.prevent="addUser">
      <h1>Thêm mới tài khoản</h1>
      <div>
        <label for="ten">Tên</label><br />
        <input type="text" id="ten" v-model="name" />
      </div>
      <div>
        <label for="email">Email</label><br />
        <input type="text" id="email" v-model="email" />
      </div>
      <div>
        <label for="matkhau">Mật khẩu</label><br />
        <input type="password" id="matkhau" v-model="password" />
      </div>
      <div>
        <label for="nhaplaimatkhau">Nhập lại mật khẩu</label><br />
        <input type="password" id="nhaplaimatkhau" v-model="rePassword" />
      </div>
      <button type="submit">Thêm mới</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const name = ref("");
const email = ref("");
const password = ref("");
const rePassword = ref("");

function addUser() {
    const user = {
        name: name.value,
        email: email.value,
        password: password.value,
        rePassword: rePassword.value
    }
    console.log(user);
    
}
</script>

<style>
</style>
