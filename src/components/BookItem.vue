<script setup>
import { ref } from 'vue'

const props = defineProps({
  book: Object
})

const emit = defineEmits([
  'delete',
  'update'
])

const isEditing = ref(false)

const editedTitle = ref(props.book.title)
const editedAuthor = ref(props.book.author)

function updateBook() {
  if (
    editedTitle.value.trim() === '' ||
    editedAuthor.value.trim() === ''
  ) {
    return
  }

  emit('update', {
    ...props.book,
    title: editedTitle.value,
    author: editedAuthor.value
  })

  isEditing.value = false
}

function toggleRead() {
  emit('update', {
    ...props.book,
    read: !props.book.read
  })
}

function deleteBook() {
  emit('delete', props.book.id)
}

function startEditing() {
  editedTitle.value = props.book.title
  editedAuthor.value = props.book.author
  isEditing.value = true
}
</script>

<template>
  <div class="book-item">

    <div v-if="!isEditing">

      <input
        type="checkbox"
        :checked="book.read"
        @change="toggleRead"
      />

      <span :class="{ read: book.read }">
        {{ book.title }}
      </span>

      <span>
        - {{ book.author }}
      </span>

      <button @click="startEditing">
        Edit
      </button>

      <button @click="deleteBook">
        Delete
      </button>

    </div>

    <div v-else>

      <input
        v-model="editedTitle"
        placeholder="Book title"
        @keyup.enter="updateBook"
      />

      <input
        v-model="editedAuthor"
        placeholder="Author"
        @keyup.enter="updateBook"
      />

      <button @click="updateBook">
        Save
      </button>

      <button @click="isEditing = false">
        Cancel
      </button>

    </div>

  </div>
</template>

<style scoped>
.book-item {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.book-item span {
  margin: 0 5px;
}

.read {
  text-decoration: line-through;
  color: gray;
}

button {
  margin-left: 5px;
}
</style>