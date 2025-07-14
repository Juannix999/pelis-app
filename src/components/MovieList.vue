<template>
  <div class="movie-list">
    <div v-for="movie in movies" :key="movie.id" class="card">
      <h3>{{ movie.title }}</h3>
      <p><strong>Fecha de estreno:</strong> {{ movie.release_date }}</p>
      <p><strong>Resumen:</strong> {{ movie.overview || 'Sin descripción.' }}</p>
      <button @click="$emit('add', movie)">➕ Agregar a favoritos</button>
    </div>
  </div>
</template>

<script setup>
defineProps(['movies'])
const emit = defineEmits(['add'])
</script>

<style scoped>
.movie-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1rem;
}
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 1rem;
  background-color: #fff;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
}
button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 0.4rem 0.8rem;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #2980b9;
}
</style>

  