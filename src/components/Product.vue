<template>
  <div>
    <div class="nav-bar"></div>

    <div class="product">
      <div class="product-image">
        <img :src="image" :alt="alt" />
      </div>

      <div class="product-info">
        <h1>{{ title }}</h1>
        <p v-if="inStock">In Stock</p>

        <p v-else :class="{out: !inStock}">Out of Stock</p>

        <p>Shipping: {{shipping}}</p>

        <ul>
          <li v-for="detail in details" v-bind:key="detail">{{detail}}</li>
        </ul>

        <div
          v-for="(variant, index) in variants"
          v-bind:key="variant.variantID"
          class="color-box"
          :style="{ backgroundColor: variant.variantColor }"
          @mouseover="updateProduct(index)"
        ></div>

        <button
          v-on:click="addToCart"
          v-bind:disabled="!inStock"
          :class="{ disabledButton: !inStock }"
        >Add to Cart</button>
      </div>
    </div>

    <ProductTabs :reviews="reviews" />
  </div>
</template>

<script>
import Vue from "vue";
var eventBus = new Vue();

import ProductTabs from "./ProductTabs";

export default {
  name: "Product",
  components: {
    ProductTabs
  },
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      brand: "Vue Mastery",
      product: "Socks",
      selectedVariant: 0,
      alt: "Photo of Vue socks",
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          variantID: 2234,
          variantColor: "green",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
          variantQuantity: 10
        },
        {
          variantID: 2235,
          variantColor: "blue",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
          variantQuantity: 0
        }
      ],
      reviews: []
    };
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].variantID);
    },
    updateProduct(index) {
      this.selectedVariant = index;
    }
  },
  computed: {
    title() {
      return `${this.brand} ${this.product}`;
    },
    image() {
      return this.variants[this.selectedVariant].variantImage;
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity;
    },
    shipping() {
      if (this.premium) {
        return "Free";
      } else {
        return 2.99;
      }
    }
  },
  mounted() {
    eventBus.$on("review-submitted", productReview => {
      this.reviews.push(productReview);
    });
  }
};
</script>

<style scoped>
</style>