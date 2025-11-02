<template>
  <div :class="['product-card', categoryClass]">
    <!-- Page Number Indicator -->
    <div class="page-indicator">{{ currentIndex }}/{{ totalProducts }}</div>
    
    <template v-if="isAvailable">
      <div class="product-content">
        <img :src="product.image" :alt="product.title" />
        <div class="product-info">
          <p class="category">{{ product.category }}</p>
          <h2>{{ product.title }}</h2>

          <div class="rating-row">
            <p class="rating-text">{{ formatRating(product.rating?.rate) }}/5</p>
            <div class="color-dots">
              <span class="color-dot" :class="getRatingClass(0)"></span>
              <span class="color-dot" :class="getRatingClass(1)"></span>
              <span class="color-dot" :class="getRatingClass(2)"></span>
              <span class="color-dot" :class="getRatingClass(3)"></span>
              <span class="color-dot" :class="getRatingClass(4)"></span>
            </div>
          </div>

          <p class="description">{{ truncateDescription(product.description) }}</p>

          <p class="price">${{ formatPrice(product.price) }}</p>

          <div class="button-group">
            <button class="buy-btn">Buy now</button>
            <button class="next-btn" @click="$emit('next')">Next product</button>
          </div>
        </div>
      </div>
    </template>

    <template v-else>
      <div class="unavailable-content">
        <div class="sad-face-icon">
          <div class="eyes">
            <div class="eye left"></div>
            <div class="eye right"></div>
          </div>
          <div class="mouth"></div>
        </div>
        <div class="unavailable-text-content">
          <p class="message">This product is unavailable to show</p>
          <button class="next-btn" @click="$emit('next')">Next product</button>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  props: ["product", "currentIndex", "totalProducts"],
  computed: {
    isAvailable() {
      return this.product.category === "men's clothing" || this.product.category === "women's clothing";
    },
    categoryClass() {
      if (this.product.category === "men's clothing") return "men";
      if (this.product.category === "women's clothing") return "women";
      return "unavailable";
    },
  },
  methods: {
    truncateDescription(description) {
      if (!description) return "";
      return description.length > 200 ? description.substring(0, 200) + "..." : description;
    },
    getRatingClass(index) {
      const rating = this.product.rating?.rate || 0;
      return rating >= index + 1 ? "filled" : "empty";
    },
    formatRating(rate) {
      return rate ? rate.toFixed(1) : "0.0";
    },
    formatPrice(price) {
      return price ? price.toFixed(2) : "0.00";
    },
  },
};
</script>

<style scoped src="../assets/style/page.css"></style>
