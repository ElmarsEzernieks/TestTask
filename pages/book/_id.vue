<template>
  <div class="container">
    <nuxt-link to="/" class="back-to-books"> Explore </nuxt-link>
    <div class="book-section-wrapper">
      <div class="book-section">
        <div class="book-info">
          <BookCard :book="book" size="large" showPublisher />
          <div class="average-rating">
            <u>{{ book.rating.toFixed(1) }}</u>
            <div class="star-container">
              <img
                v-for="index in 5"
                :key="index"
                :src="index <= Math.round(book.rating) ? filledStar : emptyStar"
              />
            </div>
          </div>
        </div>
        <p class="book-description">{{ book.description }}</p>
      </div>
    </div>
    <ReviewForm :bookId="book.id" />
  </div>
</template>

<script>
import BookCard from "~/components/BookCard.vue";
import ReviewForm from "~/components/ReviewForm.vue";
import filledStar from "~/assets/filledStar.svg";
import emptyStar from "~/assets/emptyStar.svg";

export default {
  components: {
    BookCard,
    ReviewForm,
  },
  data() {
    return {
      book: {},
      filledStar,
      emptyStar,
    };
  },
  async asyncData({ params }) {
    const { id } = params;
    const books = require("~/static/books.json").books;
    const book = books.find((book) => book.id === Number(id));
    return { book };
  },
  head() {
    return {
      title: this.book.title,
    };
  },
};
</script>

<style scoped>
@import "./_id.css";
</style>
