<script setup>
import { ref, onMounted } from 'vue'
import BookItem from './components/BookItem.vue'

const books = ref([])

const newTitle = ref('')
const newAuthor = ref('')

function addBook() {
  if (
    newTitle.value.trim() === '' ||
    newAuthor.value.trim() === ''
  ) {
    return
  }

  books.value.push({
    id: Date.now(),
    title: newTitle.value,
    author: newAuthor.value,
    read: false
  })

  newTitle.value = ''
  newAuthor.value = ''

  saveBooks()
}

function deleteBook(id) {
  books.value = books.value.filter(
    book => book.id !== id
  )

  saveBooks()
}

function updateBook(updatedBook) {
  const index = books.value.findIndex(
    book => book.id === updatedBook.id
  )

  if (index !== -1) {
    books.value[index] = updatedBook
  }

  saveBooks()
}

function saveBooks() {
  localStorage.setItem(
    'readingList',
    JSON.stringify(books.value)
  )
}

onMounted(() => {
  const savedBooks = localStorage.getItem('readingList')

  if (savedBooks) {
    books.value = JSON.parse(savedBooks)
  }
})
</script>

<template>
  <div class="container">

    <h1>My Reading List</h1>

    <p>
      Keep track of the books you want to read.
    </p>

    <div class="add-book">

      <input
        v-model="newTitle"
        type="text"
        placeholder="Enter book title"
        @keyup.enter="addBook"
      />

      <input
        v-model="newAuthor"
        type="text"
        placeholder="Enter author"
        @keyup.enter="addBook"
      />

      <button @click="addBook">
        Add Book
      </button>

    </div>

    <div class="book-list">

      <BookItem
        v-for="book in books"
        :key="book.id"
        :book="book"
        @delete="deleteBook"
        @update="updateBook"
      />

    </div>

    <p v-if="books.length === 0">
      No books in your reading list yet.
    </p>

  </div>
</template>

<style>
.container {
  width: 600px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
}

p {
  text-align: center;
  color: gray;
}

.add-book {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-book input {
  flex: 1;
  padding: 10px;
}

button {
  padding: 10px 15px;
  cursor: pointer;
}

.book-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>