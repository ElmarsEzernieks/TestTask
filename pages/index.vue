<template>
  <div class="container">
    <h1 class="explore-text">Explore books</h1>
    <div class="search-bar">
      <input
        v-model="searchQuery"
        class="search-input"
        placeholder="Search for books..."
      />
    </div>
    <div v-if="searchQuery">
      <p class="result-text" v-if="resultCount > 0">
        Results: {{ resultCount }}
      </p>
      <div class="book-container">
        <div class="book-grid">
          <BookCard
            v-for="book in filteredBooks"
            :key="book.id"
            :book="book"
            size="small"
            @click="navigateToBookDetail"
          />
        </div>
      </div>
    </div>
    <img
      v-else
      src="/images/TempBook.png"
      alt="Default Image"
      class="default-image"
    />
  </div>
</template>

<script>
import BookCard from "~/components/BookCard.vue";

export default {
  components: {
    BookCard,
  },
  data() {
    return {
      searchQuery: "",
      books: require("~/static/books.json").books,
    };
  },
  methods: {
    navigateToBookDetail(bookId) {
      this.$router.push(`/book/${bookId}`);
    },
  },
  computed: {
    filteredBooks() {
      const query = this.searchQuery.toLowerCase();
      return this.books.filter(
        (book) =>
          book.title.toLowerCase().includes(query) ||
          book.isbn.toLowerCase().includes(query) ||
          book.author.toLowerCase().includes(query)
      );
    },
    resultCount() {
      return this.filteredBooks.length;
    },
    rows() {
      const books = this.filteredBooks;
      const rowSize = 3;
      const numRows = Math.ceil(books.length / rowSize);
      const rows = [];

      for (let i = 0; i < numRows; i++) {
        const start = i * rowSize;
        const end = start + rowSize;
        rows.push(books.slice(start, end));
      }

      return rows;
    },
  },
};
</script>

<style scoped>
@import "./index.css";
</style>
