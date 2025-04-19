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

