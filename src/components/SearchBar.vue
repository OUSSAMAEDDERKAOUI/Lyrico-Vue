<template>
  <div class="search-container">
    <div class="search-box">
      <input 
        type="text" 
        v-model="searchTerm"
        @keyup.enter="handleSearch"
        placeholder="Rechercher une chanson ou un artiste..."
        :disabled="isLoading"
      />
      <button 
        @click="handleSearch" 
        :disabled="isLoading"
        class="search-button"
      >
        {{ isLoading ? 'Recherche...' : 'Rechercher' }}
      </button>
    </div>
    <div class="search-tips">
      Astuce: Vous pouvez rechercher par titre de chanson ou nom d'artiste
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  isLoading: Boolean
});

const emit = defineEmits(['search']);

const searchTerm = ref('');

function handleSearch() {
  if (searchTerm.value.trim() && !props.isLoading) {
    emit('search', searchTerm.value);
  }
}
</script>

