<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <button v-on:click="createProduct()">Create Product</button>
    <p>Name:<input type="text" v-model="name"></p>
    <p>Price:<input type="text" v-model="price"></p>
    <p>Description:<input type="text" v-model="description"></p>
    <p>Image_url:<input type="text" v-model="image_url"></p>

    <div v-for="product in products">
      <h3>{{ product.name }}</h3>
      <p><img v-bind:src="product.image_url" v-bind:alt="product.name"></p>
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
      name: "",
      price: "",
      description: "",
      image_url: "",
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
    createProduct: function () {
      console.log("create function");
      var params = {
        name: this.name,
        price: this.price,
        description: this.description,
        image_url: this.image_url,
      };
      axios.post("/api/products", params).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },
  },
};
</script>