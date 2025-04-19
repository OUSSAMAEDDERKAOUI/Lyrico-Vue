<template>
  <div class="song-details-overlay">
    <div class="song-details-modal">
      <div class="modal-header">
        <button class="close-button" @click="$emit('close')">×</button>
        <h2>{{ song.title }}</h2>
        <h3>{{ song.artist.name }}</h3>
      </div>
      
      <div class="modal-content">
        <div class="song-meta">
          <div class="album-cover">
            <img :src="song.album.cover_big || song.album.cover_medium || '/placeholder-album.jpg'" :alt="song.album.title">
          </div>
          
          <div class="song-info">
            <p><strong>Album:</strong> {{ song.album.title }}</p>
            <p v-if="song.explicit_lyrics"><strong>Contenu explicite:</strong> Oui</p>
            <p v-if="song.duration"><strong>Durée:</strong> {{ formatDuration(song.duration) }}</p>
            
            <div class="audio-preview" v-if="song.preview">
              <h4>Écouter un extrait</h4>
              <AudioPlayer :audioUrl="song.preview" :songTitle="song.title" />
            </div>
          </div>
        </div>
        
        <div class="lyrics-container">
          <h3>Paroles</h3>
          
          <div v-if="isLoadingLyrics" class="loading-lyrics">
            Chargement des paroles...
          </div>
          
          <div v-else-if="lyrics" class="lyrics-content">
            <pre>{{ lyrics }}</pre>
          </div>
          
          <div v-else class="no-lyrics">
            Paroles non disponibles pour cette chanson.
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import AudioPlayer from './AudioPlayer.vue';

const props = defineProps({
  song: {
    type: Object,
    required: true
  },
  lyrics: {
    type: String,
    default: ''
  },
  isLoadingLyrics: {
    type: Boolean,
    default: false
  }
});

defineEmits(['close']);

function formatDuration(seconds) {
  const minutes = Math.floor(seconds / 60);
  const remainingSeconds = seconds % 60;
  return `${minutes}:${remainingSeconds < 10 ? '0' : ''}${remainingSeconds}`;
}
</script>

<style scoped>
.song-details-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.song-details-modal {
  background-color: white;
  width: 90%;
  max-width: 800px;
  max-height: 90vh;
  border-radius: 8px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.modal-header {
  padding: 15px 20px;
  background-color: #42b983;
  color: white;
  position: relative;
}

.modal-header h2 {
  margin: 0;
  font-size: 24px;
  color: white;
  border-bottom: none;
}

.modal-header h3 {
  margin: 5px 0 0 0;
  font-size: 18px;
  font-weight: normal;
  opacity: 0.9;
}

.close-button {
  position: absolute;
  top: 15px;
  right: 20px;
  background: none;
  border: none;
  color: white;
  font-size: 28px;
  cursor: pointer;
}

.modal-content {
  padding: 20px;
  overflow-y: auto;
}

.song-meta {
  display: flex;
  margin-bottom: 30px;
}

.album-cover {
  width: 200px;
  height: 200px;
  margin-right: 30px;
  overflow: hidden;
  border-radius: 6px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.album-cover img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.song-info {
  flex: 1;
}

.song-info p {
  margin: 8px 0;
}

.audio-preview {
  margin-top: 20px;
}

.audio-preview h4 {
  margin-bottom: 10px;
}

.lyrics-container {
  margin-top: 20px;
  border-top: 1px solid #eee;
  padding-top: 20px;
}

.lyrics-container h3 {
  margin-top: 0;
  margin-bottom: 20px;
}

.lyrics-content {
  white-space: pre-wrap;
  line-height: 1.6;
}

pre {
  font-family: inherit;
  margin: 0;
  white-space: pre-wrap;
}

.loading-lyrics, .no-lyrics {
  padding: 20px;
  text-align: center;
  font-style: italic;
  color: #777;
}
</style>