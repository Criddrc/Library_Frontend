<template>
  <div class="books-container">
    <h2>Books</h2>
    <form @submit.prevent="addBook" class="book-form">
      <input
        v-model="form.nombre"
        placeholder="Nombre"
        required
      />
      <input
        v-model="form.genero"
        placeholder="Género"
        required
      />
      <input
        v-model.number="form.añoPublicacion"
        type="number"
        placeholder="Año"
        required
      />
      <select
        v-model.number="form.author.id"
        required
      >
        <option disabled value="0">Selecciona autor</option>
        <option v-for="w in writers" :key="w.id" :value="w.id">
          {{ w.nombre }} {{ w.apellido }}
        </option>
      </select>
      <button type="submit">Agregar libro</button>
    </form>
    <ul class="book-list">
      <li v-for="b in books" :key="b.id">
        "{{ b.nombre }}" por {{ b.author.nombre }} {{ b.author.apellido }}
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { getBooks, createBook, getWriters, Book, Writer } from '../services/api';

const books = ref<Book[]>([]);
const writers = ref<Writer[]>([]);
const form = ref({
  nombre: '',
  genero: '',
  añoPublicacion: 0,
  author: { id: 0 }
});

const fetchWriters = async () => {
  writers.value = await getWriters();
};
const fetchBooks = async () => {
  books.value = await getBooks();
};
const addBook = async () => {
  await createBook(form.value);
  form.value = { nombre: '', genero: '', añoPublicacion: 0, author: { id: 0 } };
  fetchBooks();
};

onMounted(() => {
  fetchWriters();
  fetchBooks();
});
</script>

<style scoped>
.books-container {
  max-width: 400px;
  margin: 30px auto;
  padding: 24px;
  background: #f8f9fa;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}

h2 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 18px;
}

.book-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.book-form input,
.book-form select {
  padding: 8px;
  border: 1px solid #bdbdbd;
  border-radius: 6px;
  font-size: 1em;
}

.book-form button {
  background: #1976d2;
  color: #fff;
  border: none;
  border-radius: 6px;
  padding: 10px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.2s;
}

.book-form button:hover {
  background: #1565c0;
}

.book-list {
  margin-top: 24px;
  padding-left: 18px;
}

.book-list li {
  margin-bottom: 8px;
  color: #333;
}
</style>