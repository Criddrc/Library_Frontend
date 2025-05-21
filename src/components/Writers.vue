<template>
  <div class="writers-container">
    <h2>Writers</h2>
    <form @submit.prevent="addWriter" class="writer-form">
      <input
        v-model="form.nombre"
        placeholder="Nombre"
        required
      />
      <input
        v-model="form.apellido"
        placeholder="Apellido"
        required
      />
      <input
        v-model="form.nacionalidad"
        placeholder="Nacionalidad"
        required
      />
      <input
        v-model.number="form.edad"
        type="number"
        placeholder="Edad"
        required
      />
      <button type="submit">Agregar escritor</button>
    </form>
    <ul class="writer-list">
      <li v-for="w in writers" :key="w.id">
        {{ w.nombre }} {{ w.apellido }} ({{ w.nacionalidad }}, {{ w.edad }} años)
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { getWriters, createWriter, Writer } from '../services/api';

const writers = ref<Writer[]>([]);
const form = ref({ nombre: '', apellido: '', nacionalidad: '', edad: 0 });

const fetchWriters = async () => {
  writers.value = await getWriters();
};

const addWriter = async () => {
  await createWriter(form.value);
  form.value = { nombre: '', apellido: '', nacionalidad: '', edad: 0 };
  fetchWriters();
};

onMounted(fetchWriters);
</script>

<style scoped>
.writers-container {
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

.writer-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.writer-form input {
  padding: 8px;
  border: 1px solid #bdbdbd;
  border-radius: 6px;
  font-size: 1em;
}

.writer-form button {
  background: #1976d2;
  color: #fff;
  border: none;
  border-radius: 6px;
  padding: 10px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.2s;
}

.writer-form button:hover {
  background: #1565c0;
}

.writer-list {
  margin-top: 24px;
  padding-left: 18px;
}

.writer-list li {
  margin-bottom: 8px;
  color: #333;
}
</style>