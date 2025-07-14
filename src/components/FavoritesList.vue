<template>
  <div class="favorites">
    <div v-for="movie in favorites" :key="movie.id" class="card">
      <h3>{{ movie.title }}</h3>
      <p><strong>Nota:</strong></p>
      <textarea v-model="movie.note" @blur="emitNote(movie.id, movie.note)" placeholder="Agrega una nota..."></textarea>
      <p><strong>Estado:</strong> {{ movie.status === 'seen' ? 'Ya vista' : 'Quiero ver' }}</p>
      <button @click="$emit('toggle-status', movie.id)">
        {{ movie.status === 'seen' ? 'Marcar como "Quiero ver"' : 'Marcar como "Vista"' }}
      </button>
      <button class="remove" @click="$emit('remove', movie.id)">❌ Quitar</button>
    </div>
  </div>
</template>

<script setup>
defineProps(['favorites'])
const emit = defineEmits(['remove', 'update-note', 'toggle-status'])

const emitNote = (id, note) => {
  emit('update-note', id, note)
}
</script>

<style scoped>
.favorites {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 1rem;
  background-color: #f9f9f9;
}
textarea {
  width: 100%;
  min-height: 60px;
  margin-bottom: 0.5rem;
}
button {
  margin-right: 0.5rem;
  padding: 0.3rem 0.7rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button.remove {
  background-color: #e74c3c;
  color: white;
}
</style>
