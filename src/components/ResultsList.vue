<template>
  <div class="results-container">
    <h2>Résultats de recherche ({{ results.length }})</h2>
    
    <ul class="results-list">
      <li v-for="song in results" :key="song.id" class="result-item">
        <div class="song-info">
          <div class="album-cover">
            <img :src="song.album.cover_medium || '/placeholder-album.jpg'" :alt="song.album.title">
          </div>
          
          <div class="song-details">
            <h3>{{ song.title }}</h3>
            <p class="artist">{{ song.artist.name }}</p>
            <p class="album">Album: {{ song.album.title }}</p>
          </div>
        </div>
        
        <div class="song-actions">
          <button @click="$emit('select-song', song)" class="view-lyrics-btn">
            Voir les paroles
          </button>
          
          <AudioPlayer v-if="song.preview" :audioUrl="song.preview" :songTitle="song.title" />
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import AudioPlayer from './AudioPlayer.vue';

defineProps({
  results: {
    type: Array,
    required: true
  }
});

defineEmits(['select-song']);
</script>

<style scoped>
.results-container {
  margin-top: 20px;
}

h2 {
  margin-bottom: 20px;
  color: #333;
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
}

.results-list {
  list-style: none;
  padding: 0;
}

.result-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #eee;
  transition: background-color 0.2s;
}

.result-item:hover {
  background-color: #f9f9f9;
}

.song-info {
  display: flex;
  align-items: center;
  flex: 3;
}

.album-cover {
  width: 80px;
  height: 80px;
  margin-right: 20px;
  overflow: hidden;
  border-radius: 4px;
}

.album-cover img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.song-details {
  flex: 1;
}

.song-details h3 {
  margin: 0 0 5px 0;
  color: #333;
}

.artist {
  font-weight: bold;
  margin: 0 0 5px 0;
  color: #555;
}

.album {
  margin: 0;
  color: #777;
  font-size: 14px;
}

.song-actions {
  display: flex;
  flex-direction: column;
  gap: 10px;
  flex: 1;
  align-items: flex-end;
}

.view-lyrics-btn {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.view-lyrics-btn:hover {
  background-color: #3aa876;
}
</style>