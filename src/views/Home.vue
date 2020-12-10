<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div v-for="product in products">
      <h3>{{ product.name }}</h3>
      <img v-bind:src="product.image_url">
      <p>${{ product.price }}</p>
    </div>
    <!-- <button v-on:click="productsIndex">Index</button> -->

  </div>
</template>
<style>
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
    };
  },
  created: function () {
    console.log("in created");
    this.productsIndex();
  },
  methods: {
    productsIndex: function () {
      console.log("products index......");
      axios.get("/api/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
  },
};
</script>