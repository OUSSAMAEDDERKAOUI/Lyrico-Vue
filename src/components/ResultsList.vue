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

