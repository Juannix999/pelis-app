<template>
  <div class="app">
    <h1>🎬 Pelis App – Recomendaciones de Cine</h1>

    <section class="peliculas-populares">
      <h2>Películas Populares</h2>
      <div v-if="peliculas.length === 0">Cargando películas...</div>
      <div class="grid">
        <div v-for="peli in peliculas" :key="peli.id" class="peli-card">
          <img :src="getImageUrl(peli.poster_path)" :alt="peli.title" />
          <h3>{{ peli.title }}</h3>
          <button @click="agregarAFavoritos(peli)" :disabled="isEnFavoritos(peli.id)">Agregar a Favoritos</button>
          <button @click="agregarAWatchlist(peli)" :disabled="isEnWatchlist(peli.id)">Agregar a Quiero Ver</button>
        </div>
      </div>
    </section>

    <section class="lista-favoritos">
      <h2>Favoritos</h2>
      <div v-if="favoritos.length === 0">No tienes películas favoritas aún.</div>
      <div class="grid">
        <div v-for="peli in favoritos" :key="peli.id" class="peli-card">
          <img :src="getImageUrl(peli.poster_path)" :alt="peli.title" />
          <h3>{{ peli.title }}</h3>
          <textarea
            v-model="peli.nota"
            placeholder="Agregar/editar nota personal"
            @input="editarNotaFavorito(peli.id, peli.nota)"
          ></textarea>
          <button @click="eliminarDeFavoritos(peli.id)">Eliminar</button>
        </div>
      </div>
    </section>

    <section class="lista-watchlist">
      <h2>Películas que Quiero Ver</h2>
      <div v-if="watchlist.length === 0">No tienes películas en tu lista de 'Quiero Ver'.</div>
      <div class="grid">
        <div v-for="peli in watchlist" :key="peli.id" class="peli-card">
          <img :src="getImageUrl(peli.poster_path)" :alt="peli.title" />
          <h3>{{ peli.title }}</h3>
          <textarea
            v-model="peli.nota"
            placeholder="Agregar/editar nota personal"
            @input="editarNotaWatchlist(peli.id, peli.nota)"
          ></textarea>
          <button @click="eliminarDeWatchlist(peli.id)">Eliminar</button>
          <button @click="moverWatchlistAFavoritos(peli.id)">Marcar como visto</button>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// Estados reactivos
const peliculas = ref([])
const favoritos = ref(JSON.parse(localStorage.getItem('favoritos')) || [])
const watchlist = ref(JSON.parse(localStorage.getItem('watchlist')) || [])

// API TMDb
const API_KEY = import.meta.env.VITE_TMDB_KEY
const URL_POPULAR = `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=es-ES&page=1`

async function cargarPeliculas() {
  try {
    const res = await fetch(URL_POPULAR)
    const data = await res.json()
    peliculas.value = data.results
  } catch (error) {
    console.error('Error cargando películas:', error)
  }
}

// Función para obtener URL completa del poster
function getImageUrl(path) {
  return path ? `https://image.tmdb.org/t/p/w300${path}` : 'https://via.placeholder.com/300x450?text=No+Image'
}

// Funciones Favoritos
function agregarAFavoritos(peli) {
  if (!favoritos.value.find(f => f.id === peli.id)) {
    favoritos.value.push({...peli, nota: ''})
    actualizarStorage('favoritos', favoritos.value)
    eliminarDeWatchlist(peli.id)
  }
}

function eliminarDeFavoritos(id) {
  favoritos.value = favoritos.value.filter(f => f.id !== id)
  actualizarStorage('favoritos', favoritos.value)
}

function editarNotaFavorito(id, nuevaNota) {
  const peli = favoritos.value.find(f => f.id === id)
  if (peli) {
    peli.nota = nuevaNota
    actualizarStorage('favoritos', favoritos.value)
  }
}

function isEnFavoritos(id) {
  return favoritos.value.some(f => f.id === id)
}

// Funciones Watchlist
function agregarAWatchlist(peli) {
  if (!watchlist.value.find(w => w.id === peli.id)) {
    watchlist.value.push({...peli, nota: ''})
    actualizarStorage('watchlist', watchlist.value)
  }
}

function eliminarDeWatchlist(id) {
  watchlist.value = watchlist.value.filter(w => w.id !== id)
  actualizarStorage('watchlist', watchlist.value)
}

function editarNotaWatchlist(id, nuevaNota) {
  const peli = watchlist.value.find(w => w.id === id)
  if (peli) {
    peli.nota = nuevaNota
    actualizarStorage('watchlist', watchlist.value)
  }
}

function isEnWatchlist(id) {
  return watchlist.value.some(w => w.id === id)
}

// Mover película de watchlist a favoritos
function moverWatchlistAFavoritos(id) {
  const peli = watchlist.value.find(w => w.id === id)
  if (peli) {
    agregarAFavoritos(peli)
    eliminarDeWatchlist(id)
  }
}

// Guardar en localStorage
function actualizarStorage(clave, valor) {
  localStorage.setItem(clave, JSON.stringify(valor))
}

onMounted(() => {
  cargarPeliculas()
})
</script>

<style scoped>
.app {
  max-width: 1000px;
  margin: auto;
  padding: 1rem;
  font-family: Arial, sans-serif;
}

h1, h2 {
  text-align: center;
}

.grid {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
}

.peli-card {
  border: 1px solid #ccc;
  border-radius: 6px;
  padding: 0.5rem;
  width: 180px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.peli-card img {
  width: 100%;
  border-radius: 4px;
}

.peli-card h3 {
  font-size: 1rem;
  margin: 0.5rem 0;
  text-align: center;
}

.peli-card button {
  margin: 0.25rem 0;
  padding: 0.3rem 0.5rem;
  font-size: 0.9rem;
  cursor: pointer;
}

.peli-card textarea {
  width: 100%;
  height: 60px;
  margin-bottom: 0.5rem;
  resize: none;
  font-size: 0.9rem;
  padding: 0.25rem;
}
</style>
