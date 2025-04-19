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

