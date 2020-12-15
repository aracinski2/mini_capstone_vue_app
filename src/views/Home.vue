<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <p>Name:<input type="text" v-model="name"></p>
    <p>Price:<input type="text" v-model="price"></p>
    <p>Description:<input type="text" v-model="description"></p>
    <p>Image_url:<input type="text" v-model="image_url"></p>
    <button v-on:click="createProduct()">Create Product</button>

    <div v-for="product in products">
      {{ product.id }}
      <h3>{{ product.name }}</h3>
      <p><img v-bind:src="product.image_url" v-bind:alt="product.name"></p>
      <p>${{ product.price }}</p>
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h3>Hello there</h3>
        <p>Name: <input type="text" v-model="currentProduct.name"></p>
        <p>Price: <input type="text" v-model="currentProduct.price"></p>
        <p>Description: <input type="text" v-model="currentProduct.description"></p>
        <p>Image url: <input type="text" v-model="currentProduct.image_url"></p>
        <button>Close</button>
        <button v-on:click="updateProduct()">Update</button>
      </form>
    </dialog>
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
      currentProduct: {},
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
    showProduct: function (theProduct) {
      console.log("theProduct");
      this.currentProduct = theProduct;
      console.log("show product");
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function () {
      var params = {
        name: this.currentProduct.name,
        price: this.currentProduct.price,
        description: this.currentProduct.description,
        image_url: this.currentProduct.image_url,
      };
      axios
        .patch("/api/products/" + this.currentProduct.id, params)
        .then((response) => {
          console.log(response.data);
        });
    },
  },
};
</script>