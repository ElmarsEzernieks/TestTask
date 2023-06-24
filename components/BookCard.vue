<template>
  <nuxt-link :to="`/book/${book.id}`" class="book-card-link">
    <div :class="['book-card', size]" @click="$emit('click', book.id)">
      <div :class="['image-container', size]">
        <img :src="book.image" :alt="book.title" class="book-image" />
      </div>
      <div class="flex-container">
        <div class="book-details">
          <div class="text-container">
            <h2 class="book-title">{{ book.title }}</h2>
            <p class="book-author">{{ book.author }}</p>
            <p v-if="publishDate" class="book-year">{{ publishDate }}</p>
            <p class="book-price">{{ book.price }}</p>
            <p v-if="showPublisher" class="book-publisher">
              {{ book.publisher }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </nuxt-link>
</template>

<script>
import axios from "axios";

export default {
  props: {
    book: {
      type: Object,
      required: true,
    },
    size: {
      type: String,
      default: "small",
    },
    showPublisher: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      publishDate: null,
    };
  },
  async created() {
    try {
      const response = await axios.get(
        `https://www.googleapis.com/books/v1/volumes?q=isbn:${this.book.isbn}&key=AIzaSyClRBJ7iEWgh6beSjXiB6q9mM2KdDjqvsw`
      );

      if (response.data.items && response.data.items.length > 0) {
        this.publishDate =
          response.data.items[0].volumeInfo.publishedDate.split("-")[0];
      }
    } catch (error) {
      console.error("Failed to fetch book's publish date:", error);
    }
  },
};
</script>

<style scoped>
@import "./BookCard.css";
</style>
