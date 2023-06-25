<template>
    <div>
      <div v-if="!reviewSubmitted" class="input-box">
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
        <p>Thank you! Your review has been received.</p>
      </div>
      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
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
    },
    data() {
      return {
        rating: 0,
        errorMessage: "",
        comment: "",
        reviewSubmitted: false,
        filledStar,
        emptyStar,
      };
    },
    methods: {
      setRating(starNumber) {
        this.rating = starNumber;
      },
      async submitReview() {
        const headers = new Headers();
        headers.append("Content-Type", "application/json");
  
        const reviewData = {
          bookId: this.bookId,
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
  };
  </script>
  
  <style scoped>
  @import "./ReviewForm.css";
  </style>
  
  