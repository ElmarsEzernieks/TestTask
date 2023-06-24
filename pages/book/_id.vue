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
    <div class="input-box" v-if="!reviewSubmitted">
      <form @submit.prevent="submitReview">
        <div class="review-title">Write a review</div>
        <div class="rate-and-review">
          <div class="rate-book">
            <div class="rate-title">Rate the book</div>
            <div class="star-container">
              <img
                v-for="index in 5"
                :key="index"
                :src="index <= rating ? filledStar : emptyStar"
                @click="setRating(index)"
              />
            </div>
            <div class="write-review">
              <textarea
                v-model="comment"
                placeholder="Write your review..."
              ></textarea>
            </div>
            <div class="submit-review">
              <button type="submit">Submit Review</button>
            </div>
          </div>
        </div>
      </form>
    </div>
    <div v-if="reviewSubmitted" class="review-submitted">
      <p>Thank you! Review received.</p>
    </div>
    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  </div>
</template>

<script>
import BookCard from "~/components/BookCard.vue";
import filledStar from "~/assets/filledStar.svg";
import emptyStar from "~/assets/emptyStar.svg";

export default {
  components: {
    BookCard,
  },
  data() {
    return {
      rating: 0,
      errorMessage: "",
      comment: "",
      averageRating: 3, // Average rating of the book
      reviewSubmitted: false, // Track if the review has been submitted
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

  computed: {
    starRating() {
      const rating = Math.round(this.book.rating);
      const stars = [];
      for (let i = 0; i < 5; i++) {
        if (i < rating) stars.push(1);
        else stars.push(0);
      }
      return stars;
    },
  },

  methods: {
    setRating(starNumber) {
      this.rating = starNumber;
    },
    async submitReview() {
      const headers = new Headers();
      headers.append("Content-Type", "application/json");

      const reviewData = {
        bookId: this.book.id,
        rating: this.rating,
        comment: this.comment,
      };

      const options = {
        method: "POST",
        headers,
        mode: "cors",
        body: JSON.stringify(reviewData),
      };

      try {
        const response = await fetch(
          "https://enudviyvv6jkm.x.pipedream.net",
          options
        );

        if (response.ok) {
          this.rating = "";
          this.comment = "";
          this.reviewSubmitted = true;
        } else {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
      } catch (error) {
        console.error("There was a problem with your fetch operation: ", error);
        this.errorMessage = "Failed to submit review. Please try again.";
      }
    },
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
