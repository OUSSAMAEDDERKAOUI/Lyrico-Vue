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

<style scoped>
.search-container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}

.search-box {
  display: flex;
}

input {
  flex: 1;
  padding: 12px 15px;
  border: 2px solid #ddd;
  border-radius: 5px 0 0 5px;
  font-size: 16px;
  outline: none;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #42b983;
}

.search-button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 0 20px;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.search-button:hover {
  background-color: #3aa876;
}

.search-button:disabled {
  background-color: #9ed0bc;
  cursor: not-allowed;
}

.search-tips {
  margin-top: 10px;
  font-size: 14px;
  color: #777;
  text-align: left;
}
</style>