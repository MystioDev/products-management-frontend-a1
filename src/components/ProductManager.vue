<script setup>
import {
  createProduct,
  deleteProduct,
  getProducts,
  updateProduct,
} from "../api/products";
</script>

<script>
export default {
  name: "ProductManager",
  data() {
    return {
      products: [],
      newProduct: {
        name: "",
        price: 0,
        inStock: false,
      },
      selectedProduct: {
        id: -1,
        name: "",
        price: 0,
        inStock: false,
      },
    };
  },
  methods: {
    async loadProducts() {
      this.products = await getProducts();
    },
    async addProduct(product) {
      await createProduct(product);
      await this.loadProducts();
    },
    async submitCreateProduct() {
      await this.addProduct(this.newProduct);
    },
    async removeProduct(id) {
      await deleteProduct(id);
      await this.loadProducts();
    },
    selectProduct(id, name, price, inStock) {
      this.selectedProduct = {
        id: id,
        name: name,
        price: price,
        inStock: inStock,
      };
    },
    async submitEditProduct() {
      await updateProduct(this.selectedProduct.id, {
        name: this.selectedProduct.name,
        price: this.selectedProduct.price,
        inStock: this.selectedProduct.inStock,
      });
      await this.loadProducts();
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
      <thead class="p-2 w-full">
        <tr class="flex justify-between gap-8">
          <th class="">azonosító</th>
          <th class="">név</th>
          <th class="">ár</th>
          <th class="">raktáron</th>
          <th class="">Műveletek</th>
        </tr>
      </thead>
      <tbody class="p-2 w-full flex flex-col gap-2">
        <tr
          v-for="product in products"
          class="flex justify-between"
          :key="product.id"
        >
          <td class="">{{ product.id }}</td>
          <td class="">{{ product.name }}</td>
          <td class="">{{ product.price }}</td>
          <td class="">{{ product.inStock ? "Igen" : "Elfogyott" }}</td>
          <td class="flex gap-1">
            <button
              @click="
                selectProduct(
                  product.id,
                  product.name,
                  product.price,
                  product.inStock,
                )
              "
              type="button"
              class="border-2 border-blue-500 rounded-xl font-sans text-blue-500 cursor-pointer px-3 py-1"
            >
              Kiválasztás
            </button>
            <button
              @click="removeProduct(product.id)"
              type="button"
              class="border-2 border-rose-500 rounded-xl font-sans text-rose-500 cursor-pointer px-3 py-1"
            >
              Törlés
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <form
      @submit.prevent="submitCreateProduct"
      class="mt-10 flex flex-col gap-2 border-2 border-stone-200 rounded-xl p-5"
    >
      <label for="name">Név:</label>
      <input
        type="text"
        name="name"
        v-model="newProduct.name"
        class="border-2 border-stone-200 rounded-xl p-2"
      />

      <label for="price">Ár:</label>
      <input
        type="number"
        name="price"
        v-model.number="newProduct.price"
        class="border-2 border-stone-200 rounded-xl p-2"
      />

      <div class="flex gap-4">
        <label for="inStock">Raktáron van-e?</label>
        <input
          v-model="newProduct.inStock"
          type="checkbox"
          name="inStock"
          id=""
        />
      </div>

      <button
        class="w-full border-2 border-green-200 rounded-xl p-2 mt-5 text-green-200 hover:border-green-400 hover:text-green-400 cursor-pointer bg-green-900"
        type="submit"
      >
        Létrehozás
      </button>
    </form>

    <h2 class="mt-10 mb-2 text-4xl">Termék szerkesztése</h2>

    <form
      @submit.prevent="submitEditProduct"
      class="flex flex-col gap-2 border-2 border-stone-200 rounded-xl p-5"
    >
      <label for="name">Név:</label>
      <input
        type="text"
        name="name"
        v-model="selectedProduct.name"
        class="border-2 border-stone-200 rounded-xl p-2"
      />

      <label for="price">Ár:</label>
      <input
        type="number"
        name="price"
        v-model.number="selectedProduct.price"
        class="border-2 border-stone-200 rounded-xl p-2"
      />

      <div class="flex gap-4">
        <label for="inStock">Raktáron van-e?</label>
        <input
          v-model="selectedProduct.inStock"
          type="checkbox"
          name="inStock"
          id=""
        />
      </div>

      <button
        class="w-full border-2 border-blue-200 rounded-xl p-2 mt-5 text-blue-200 hover:border-blue-400 hover:text-blue-400 cursor-pointer bg-blue-900"
        type="submit"
      >
        Szerkesztésre
      </button>
    </form>
  </main>
</template>
