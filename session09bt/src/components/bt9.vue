<template>
  <div>
    <form @submit.prevent="addUser">
      <h1>Đăng kí tài khoản</h1>

      <div>
        <label for="ten">Tên</label><br />
        <input type="text" id="ten" v-model="name" ref="nameInput" />
        <span v-if="errors.name" style="color: red">{{ errors.name }}</span>
      </div>

      <div>
        <label for="email">Email</label><br />
        <input type="text" id="email" v-model="email" />
        <span v-if="errors.email" style="color: red">{{ errors.email }}</span>
      </div>

      <div>
        <label for="matkhau">Mật khẩu</label><br />
        <input type="password" id="matkhau" v-model="password" />
        <span v-if="errors.password" style="color: red">{{ errors.password }}</span>
      </div>

      <div>
        <label for="sodienthoai">Nhập số điện thoại</label><br />
        <input type="number" id="sodienthoai" v-model="phone" />
        <span v-if="errors.phone" style="color: red">{{ errors.phone }}</span>
      </div>

      <button type="submit">Đăng kí</button>
    </form>

    <p v-if="successMessage" style="color: green">{{ successMessage }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const name = ref("");
const email = ref("");
const password = ref("");
const phone = ref("");
const errors = ref({}); 
const successMessage = ref("");

const nameInput = ref(null);
onMounted(() => nameInput.value.focus());

const validateForm = () => {
  errors.value = {};

  if (!name.value) {
    errors.value.name = "Tên không được để trống";
  }
  if (!email.value) {
    errors.value.email = "Email không được để trống";
  } else if (!validateEmail(email.value)) {
    errors.value.email = "Email không hợp lệ";
  } else if (isEmailExists(email.value)) {
    errors.value.email = "Email đã được sử dụng";
  }

  if (!password.value) {
    errors.value.password = "Mật khẩu không được để trống";
  }
  if (!phone.value) {
    errors.value.phone = "Vui lòng nhập số điện thoại";
  }

  return Object.keys(errors.value).length === 0;
};

const validateEmail = (email) => {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return re.test(email);
};

const isEmailExists = (email) => {
  const users = JSON.parse(localStorage.getItem("users")) || [];
  return users.some(user => user.email === email);
};

const addUser = () => {
  if (validateForm()) {
    const user = {
      name: name.value,
      email: email.value,
      password: password.value,
      phone: phone.value
    };

    let users = JSON.parse(localStorage.getItem("users")) || [];
    users.push(user);
    localStorage.setItem("users", JSON.stringify(users));

    successMessage.value = "Đăng ký tài khoản thành công!";
    clearForm();
    
    nameInput.value.focus();
  }
};

const clearForm = () => {
  name.value = "";
  email.value = "";
  password.value = "";
  phone.value = "";
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
