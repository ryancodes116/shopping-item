<template>
  <form @submit.prevent="onSubmit" class="review-form">
    <p v-if="errors.length">
      <b>Please correct the following error(s):</b>
      <ul>
        <li v-for="error in errors" :key="error">{{error}}</li>
      </ul>
    </p>
    
    <p>
      <label for="name">Name:</label>
      <input type="text" id="name" v-model="name" />
    </p>
    <p>
      <label for="Review">Review:</label>
      <textarea id="review" v-model="review" />
    </p>
    <p>
      <label for="rating">Rating:</label>
      <select id="rating" v-model.number="rating">
        <option value="5">5</option>
        <option value="4">4</option>
        <option value="3">3</option>
        <option value="2">2</option>
        <option value="1">1</option>
      </select>
    </p>
    <input type="submit" value="Submit" />
  </form>
</template>

<script>
import Vue from "vue";
var eventBus = new Vue();

export default {
  name: "ProductReview",
  data() {
    return {
      name: null,
      review: null,
      rating: null,
      errors: []
    };
  },
  methods: {
    onSubmit() {
      this.errors = [];
      if (this.name && this.review && this.rating) {
        let productReview = {
          name: this.name,
          review: this.review,
          rating: this.rating
        };
        eventBus.$emit("review-submitted", productReview);
        (this.name = null), (this.review = null), (this.rating = null);
      } else {
        if (!this.name) this.errors.push("Name required.");
        if (!this.rating) this.errors.push("Rating required.");
        if (!this.review) this.errors.push("Review required.");
      }
    }
  }
};
</script>

<style scoped>
</style>