<script setup>
import { getProducts } from "../api/products";
</script>

<script>
export default {
  name: "ProductManager",
  data() {
    return {
      products: [],
    };
  },
  methods: {
    async loadProducts() {
      this.products = await getProducts();
    },
  },
};
</script>

<template>
  <main class="flex justify-center items-center flex-col">
    <h1 class="">Termékek Management</h1>

    <table class="flex w-full">
      <tr class="">
        <th class="">id</th>
        <th class="">name</th>
        <th class="">price</th>
        <th class="">in stock</th>
      </tr>
    </table>
  </main>
</template>
