<script setup>
import { ref } from 'vue';

const searchQuery = ref('');
const books = ref([]);

// API 호출 함수
const fetchBooks = async () => {
  if (!searchQuery.value.trim()) return;
  try {
    const response = await $fetch(
      `https://openapi.naver.com/v1/search/book.json?query=${searchQuery.value}`,
      {
        headers: {
          'X-Naver-Client-Id': 'SjSUPGk79nLn6flHRYCX',
          'X-Naver-Client-Secret': 'jwEXLlXA2x',
        },
      }
    );
    books.value = response.items;
  } catch (error) {
    console.error('API 호출 실패:', error);
  }
};

// 검색 실행 함수
const handleSearch = () => {
  fetchBooks();
};
</script>

<template>
  <div class="container">
    <h1 class="title">책 검색 서비스</h1>
    <div class="search-box">
      <input
        type="text"
        v-model="searchQuery"
        placeholder="책 제목 또는 저자 검색"
        class="search-input"
      />
      <button @click="handleSearch" class="search-button">검색</button>
    </div>
    <ul class="book-list">
      <li v-for="(book, index) in books" :key="index" class="book-item">
        <img :src="book.image" alt="책 표지" class="book-cover" />
        <div class="book-info">
          <h3 class="book-title">{{ book.title }}</h3>
          <p class="book-author">{{ book.author }}</p>
          <p class="book-publisher">{{ book.publisher }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.title {
  font-size: 2rem;
  text-align: center;
  margin-bottom: 20px;
}

.search-box {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.search-input {
  width: 70%;
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.search-button {
  padding: 10px 15px;
  font-size: 1rem;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 5px;
  margin-left: 10px;
  cursor: pointer;
}

.book-list {
  list-style: none;
  padding: 0;
}

.book-item {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
  padding: 10px;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.book-cover {
  width: 80px;
  height: 120px;
  object-fit: cover;
  margin-right: 15px;
}

.book-info {
  flex: 1;
}

.book-title {
  font-size: 1.2rem;
  margin-bottom: 5px;
}

.book-author,
.book-publisher {
  font-size: 0.9rem;
  color: #555;
}
</style>
