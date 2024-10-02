<template>
  <div>
    <form @submit.prevent="addProduct">
      <h1>Thêm mới sản phẩm</h1>
      <div>
        <label for="masp">Mã sản phẩm</label><br />
        <input type="text" id="masp" v-model="id" />
      </div>
      <div>
        <label for="tensp">Tên sản phẩm</label><br />
        <input type="text" id="tensp" v-model="name" />
      </div>
      <div>
        <label for="giasp">Giá sản phẩm</label><br />
        <input min="0" type="number" id="giasp" v-model="price" />
      </div>
      <div>
        <label for="soluongsp">Số lượng sản phẩm</label><br />
        <input min="0" type="number" id="soluongsp" v-model="quantity" />
      </div>
      <button type="submit">Thêm mới</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const id = ref("");
const name = ref("");
const price = ref(0);
const quantity = ref(0);

function addProduct() {
  const product = {
    idProduct: id.value,
    nameProduct: name.value,
    priceProduct: price.value,
    quantityProduct: quantity.value,
  };
  console.log(product);
}
</script>

<style>
</style>
