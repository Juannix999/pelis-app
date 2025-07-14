<template>
  <main>
    <h1>🎬 Películas populares</h1>

    <section v-if="movies.length">
      <div v-for="movie in movies" :key="movie.id" class="movie-card">
        <h2>{{ movie.title }}</h2>
        <p>📅 Fecha de estreno: {{ movie.release_date }}</p>
        <button @click="addToFavorites(movie)">⭐ Agregar a favoritos</button>
      </div>
    </section>

    <section v-else>
      <p>Cargando películas...</p>
    </section>

    <hr />

    <section v-if="favorites.length">
      <h2>🎉 Favoritas</h2>
      <div v-for="fav in favorites" :key="fav.id" class="favorite-card">
        <h3>{{ fav.title }}</h3>
        <textarea v-model="fav.note" placeholder="Agrega una nota"></textarea>
        <button @click="removeFromFavorites(fav.id)">❌ Quitar</button>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const API_KEY = import.meta.env.VITE_TMDB_KEY
const URL_POPULAR = `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=es-ES&page=1`

const movies = ref([])
const favorites = ref([])

// Cargar películas desde la API
onMounted(async () => {
  try {
    const res = await fetch(URL_POPULAR)
    const data = await res.json()
    movies.value = data.results
  } catch (error) {
    console.error('Error al cargar películas:', error)
  }

  // Cargar favoritos desde localStorage
  const stored = localStorage.getItem('favoritos')
  if (stored) {
    favorites.value = JSON.parse(stored)
  }
})

// Agregar a favoritos
function addToFavorites(movie) {
  if (!favorites.value.find(m => m.id === movie.id)) {
    favorites.value.push({ ...movie, note: '' })
    localStorage.setItem('favoritos', JSON.stringify(favorites.value))
  }
}

// Quitar de favoritos
function removeFromFavorites(id) {
  favorites.value = favorites.value.filter(m => m.id !== id)
  localStorage.setItem('favoritos', JSON.stringify(favorites.value))
}
</script>

<style scoped>
main {
  padding: 2rem;
  font-family: Arial, sans-serif;
}
.movie-card, .favorite-card {
  border: 1px solid #ccc;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 8px;
  background-color: #f9f9f9;
}
textarea {
  width: 100%;
  min-height: 60px;
  margin-top: 0.5rem;
}
button {
  margin-top: 0.5rem;
  padding: 0.3rem 0.7rem;
  cursor: pointer;
}
</style>
