<template>
  <div>
    <div v-if="!reviewSubmitted && !loading" class="input-box">
      <form @submit.prevent="submitReview">
        <div class="review-title">{{ writeReviewTitle }}</div>
        <div class="rate-and-review">
          <div class="rate-book">
            <div class="rate-title">{{ rateBookText }}</div>
            <div class="star-container">
              <img
                v-for="index in maxRating"
                :key="index"
                :src="index <= rating ? filledStar : emptyStar"
                @click="setRating(index)"
              />
            </div>
            <div class="write-review">
              <textarea
                v-model="comment"
                :placeholder="writeReviewPlaceholder"
              ></textarea>
            </div>
            <div class="submit-review">
              <button type="submit">{{ submitButtonText }}</button>
            </div>
          </div>
        </div>
        <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
      </form>
    </div>
    <div v-if="reviewSubmitted && !loading" class="review-submitted">
      <p>{{ reviewReceivedMessage }}</p>
    </div>
    <div v-if="loading" class="loading-container">
      <img src="~/assets/loading.gif" alt="Loading..." class="loading-gif" />
    </div>
  </div>
</template>

<script>
import filledStar from "~/assets/filledStar.svg";
import emptyStar from "~/assets/emptyStar.svg";

export default {
  props: {
    bookId: {
      type: Number,
      required: true,
    },
    writeReviewTitle: {
      type: String,
      default: "Write a review",
    },
    rateBookText: {
      type: String,
      default: "Rate the book",
    },
    writeReviewPlaceholder: {
      type: String,
      default: "Write your review...",
    },
    submitButtonText: {
      type: String,
      default: "Submit Review",
    },
  },
  data() {
    return {
      rating: 0,
      errorMessage: "",
      comment: "",
      reviewSubmitted: false,
      filledStar,
      emptyStar,
      maxRating: 5,
      loading: false,
    };
  },
  computed: {
    reviewReceivedMessage() {
      return `Thank you! ${this.reviewMessage}`;
    },
    reviewMessage() {
      return "Your review has been received.";
    },
  },
  methods: {
    setRating(starNumber) {
      this.rating = starNumber;
    },
    async submitReview() {
      if (this.rating === 0) {
        this.errorMessage =
          "Please rate the book before submitting the review.";
        return;
      }

      this.loading = true;

      setTimeout(() => {
        this.loading = false;
        this.reviewSubmitted = true;
        this.errorMessage = "";
      }, 2500);
    },
  },
};
</script>

<style scoped>
@import "@/assets/css/components/ReviewForm.css";
</style>
