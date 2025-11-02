<template>
  <div id="app">
    <ProductDisplay :product="product" :currentIndex="currentIndex + 1" :totalProducts="allProducts.length" @next="nextProduct" />
  </div>
</template>

<script>
import ProductDisplay from "./components/ProductDisplay.vue";

export default {
  name: "App",
  components: { ProductDisplay },
  data() {
    return {
      index: 1,
      product: {},
      allProducts: [],
      currentIndex: 0,
    };
  },
  methods: {
    async fetchAllProducts() {
      try {
        const res = await fetch("https://fakestoreapi.com/products");
        const data = await res.json();
        const mensProducts = data.filter((p) => p.category === "men's clothing");
        const womensProducts = data.filter((p) => p.category === "women's clothing");
        const otherProducts = data.filter((p) => p.category !== "men's clothing" && p.category !== "women's clothing");

        this.allProducts = [
          ...mensProducts,
          ...womensProducts,
          ...otherProducts.map((p) => ({
            ...p,
            category: "unavailable",
            title: "Unavailable Product",
          })),
        ];

        if (this.allProducts.length > 0) {
          this.product = this.allProducts[0];
        }
      } catch (error) {
        console.error("Error fetching products:", error);
        this.product = {
          category: "unavailable",
          title: "Error Loading Product",
        };
      }
    },
    nextProduct() {
      this.currentIndex = (this.currentIndex + 1) % this.allProducts.length;
      this.product = this.allProducts[this.currentIndex];
    },
  },
  mounted() {
    this.fetchAllProducts();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap");

body {
  margin: 0;
  padding: 0;
  background-color: #f5f5f5;
  font-family: "Inter", Arial, sans-serif;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
