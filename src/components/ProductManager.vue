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
  mounted() {
    this.loadProducts();
  },
};
</script>

<template>
  <main class="flex justify-center items-center flex-col">
    <h1 class="">Termékek Management</h1>

    <table class="flex w-full flex-col">
      <thead class="p-2">
        <tr class="flex justify-around gap-8">
          <th class="">azonosító</th>
          <th class="">név</th>
          <th class="">ár</th>
          <th class="">raktáron</th>
        </tr>
      </thead>
      <tbody class="w-full">
        <tr
          v-for="product in products"
          class="flex justify-around gap-8"
          :key="product.id"
        >
          <td class="">{{ product.id }}</td>
          <td class="">{{ product.name }}</td>
          <td class="">{{ product.price }}</td>
          <td class="">{{ product.inStock ? "Igen" : "Elfogyott" }}</td>
        </tr>
      </tbody>
    </table>

    <form
      class="mt-30 flex flex-col gap-2 border-2 border-stone-200 rounded-xl p-5"
    >
      <label for="name">Név:</label>
      <input
        type="text"
        name="name"
        class="border-2 border-stone-200 rounded-xl p-2"
      />

      <label for="price">Ár:</label>
      <input
        type="number"
        name="price"
        class="border-2 border-stone-200 rounded-xl p-2"
      />

      <div class="flex gap-4">
        <label for="inStock">Raktáron van-e?</label>
        <input type="checkbox" name="inStock" id="" />
      </div>

      <button
        class="w-full border-2 border-green-200 rounded-xl p-2 mt-5 text-green-200 hover:border-green-400 hover:text-green-400 cursor-pointer bg-green-900"
        type="submit"
      >
        Létrehozás
      </button>
    </form>
  </main>
</template>
