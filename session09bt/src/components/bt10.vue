<template>
  <div>
    <form @submit.prevent="loginUser">
      <h1>Đăng nhập</h1>

      <div>
        <label for="email">Email</label><br />
        <input type="text" id="email" v-model="email" />
        <span v-if="errors.email" style="color: red">{{ errors.email }}</span>
      </div>

      <div>
        <label for="matkhau">Mật khẩu</label><br />
        <input type="password" id="matkhau" v-model="password" />
        <span v-if="errors.password" style="color: red">{{
          errors.password
        }}</span>
      </div>

      <button type="submit">Đăng nhập</button>
    </form>

    <p v-if="successMessage" style="color: green">{{ successMessage }}</p>
    <p v-if="errorMessage" style="color: red">{{ errorMessage }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const email = ref("");
const password = ref("");
const errors = ref({});
const successMessage = ref("");
const errorMessage = ref("");

const validateForm = () => {
  errors.value = {};

  if (!email.value) {
    errors.value.email = "Email không được để trống";
  }

  if (!password.value) {
    errors.value.password = "Mật khẩu không được để trống";
  }

  return Object.keys(errors.value).length === 0;
};

const loginUser = () => {
  if (validateForm()) {
    const users = JSON.parse(localStorage.getItem("users")) || [];
    const user = users.find(
      (user) => user.email === email.value && user.password === password.value
    );

    if (user) {
      successMessage.value = "Đăng nhập thành công!";
      errorMessage.value = "";
      clearForm();
    } else {
      errorMessage.value = "Email hoặc mật khẩu không đúng";
      successMessage.value = "";
    }
  }
};

const clearForm = () => {
  email.value = "";
  password.value = "";
};
</script>

<style scoped>
form div {
  margin-bottom: 10px;
}

button {
  margin-top: 10px;
}
</style>
