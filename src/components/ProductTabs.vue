<template>
  <div>
    <span
      v-for="(tab, index) in tabs"
      :key="index"
      class="tabs"
      :class="{ activeTab: selectedTab === tab }"
      @click="selectedTab = tab"
    >{{tab}}</span>

    <div v-show="selectedTab === 'Reviews'">
      <p v-if="!reviews.length" class="no-reviews">There are no reviews yet.</p>
      <ul>
        <li v-for="review in reviews" :key="review">
          <p>{{ review.name }}</p>
          <p>Rating: {{review.rating}}</p>
          <p>{{review.review}}</p>
        </li>
      </ul>
    </div>

    <ProductReview v-show="selectedTab === 'Make a Review'" />
  </div>
</template>

<script>
import ProductReview from "./ProductReview";

export default {
  name: "ProductTabs",
  components: {
    ProductReview
  },
  data() {
    return {
      tabs: ["Reviews", "Make a Review"],
      selectedTab: "Reviews"
    };
  },
  props: {
    reviews: {
      type: Array,
      required: true
    }
  }
};
</script>

<style scoped>
</style>