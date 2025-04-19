<template>
  <div class="app-container">
    <header>
      <h1>Music Lyrics Finder</h1>
      <SearchBar @search="searchSongs" :isLoading="isLoading" />
    </header>

    <main>
      <div v-if="error" class="error-message">
        {{ error }}
      </div>

      <div v-if="isLoading" class="loading">
        Recherche en cours...
      </div>

      <div v-else-if="searchPerformed && !searchResults.length" class="no-results">
        Aucun résultat trouvé pour "{{ lastSearchTerm }}"
      </div>

      <ResultsList 
        v-else-if="searchResults.length > 0" 
        :results="searchResults" 
        @select-song="selectSong" 
      />

      <SongDetails 
        v-if="selectedSong" 
        :song="selectedSong" 
        :lyrics="lyrics" 
        :isLoadingLyrics="isLoadingLyrics"
        @close="closeSongDetails" 
      />
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import SearchBar from './components/SearchBar.vue';
import ResultsList from './components/ResultsList.vue';
import SongDetails from './components/SongDetails.vue';

// State
const searchResults = ref([]);
const selectedSong = ref(null);
const lyrics = ref('');
const isLoading = ref(false);
const isLoadingLyrics = ref(false);
const error = ref('');
const searchPerformed = ref(false);
const lastSearchTerm = ref('');

// Methods
async function searchSongs(term) {
  if (!term.trim()) {
    error.value = 'Veuillez entrer un terme de recherche';
    return;
  }

  error.value = '';
  isLoading.value = true;
  searchPerformed.value = true;
  lastSearchTerm.value = term;
  
  try {
    const response = await fetch(`https://api.lyrics.ovh/suggest/${encodeURIComponent(term)}`);
    
    if (!response.ok) {
      throw new Error(`Erreur lors de la recherche: ${response.statusText}`);
    }
    
    const data = await response.json();
    searchResults.value = data.data || [];
  } catch (err) {
    console.error('Erreur de recherche:', err);
    error.value = `Une erreur est survenue: ${err.message}`;
    searchResults.value = [];
  } finally {
    isLoading.value = false;
  }
}

async function selectSong(song) {
  selectedSong.value = song;
  isLoadingLyrics.value = true;
  lyrics.value = '';
  error.value = '';
  
  try {
    const artist = encodeURIComponent(song.artist.name);
    const title = encodeURIComponent(song.title);
    const response = await fetch(`https://api.lyrics.ovh/v1/${artist}/${title}`);
    
    if (!response.ok) {
      if (response.status === 404) {
        throw new Error('Paroles non disponibles pour cette chanson');
      }
      throw new Error(`Erreur lors de la récupération des paroles: ${response.statusText}`);
    }
    
    const data = await response.json();
    lyrics.value = data.lyrics || 'Paroles non disponibles';
  } catch (err) {
    console.error('Erreur de récupération des paroles:', err);
    lyrics.value = `Paroles non disponibles: ${err.message}`;
  } finally {
    isLoadingLyrics.value = false;
  }
}

function closeSongDetails() {
  selectedSong.value = null;
  lyrics.value = '';
}
</script>

<style scoped>
.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

header {
  text-align: center;
  margin-bottom: 30px;
}

h1 {
  color: #333;
  margin-bottom: 20px;
}

.error-message {
  background-color: #ffebee;
  color: #c62828;
  padding: 15px;
  border-radius: 5px;
  margin-bottom: 20px;
  text-align: center;
}

.loading, .no-results {
  text-align: center;
  padding: 30px;
  color: #666;
  font-style: italic;
}
</style>