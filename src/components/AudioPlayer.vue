<template>
  <div class="audio-player">
    <audio ref="audioElement" :src="audioUrl" preload="none"></audio>
    
    <div class="player-controls">
      <button class="play-button" @click="togglePlay">
        {{ isPlaying ? '❚❚' : '▶' }}
      </button>
      
      <div class="progress-container">
        <div class="progress-bar" :style="{ width: `${progress}%` }"></div>
      </div>
      
      <div class="time-display">{{ formatTime(currentTime) }} / {{ formatTime(duration) }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const props = defineProps({
  audioUrl: {
    type: String,
    required: true
  },
  songTitle: {
    type: String,
    default: 'Audio'
  }
});

const audioElement = ref(null);
const isPlaying = ref(false);
const duration = ref(0);
const currentTime = ref(0);
const progress = ref(0);

function togglePlay() {
  if (!audioElement.value) return;
  
  if (isPlaying.value) {
    audioElement.value.pause();
  } else {
    audioElement.value.play().catch(error => {
      console.error('Erreur de lecture audio:', error);
    });
  }
}

function updateProgress() {
  if (!audioElement.value) return;
  
  currentTime.value = audioElement.value.currentTime;
  duration.value = audioElement.value.duration || 0;
  
  if (duration.value > 0) {
    progress.value = (currentTime.value / duration.value) * 100;
  }
}

function formatTime(seconds = 0) {
  if (isNaN(seconds) || !isFinite(seconds)) return '0:00';
  
  const mins = Math.floor(seconds / 60);
  const secs = Math.floor(seconds % 60);
  return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
}

onMounted(() => {
  if (audioElement.value) {
    audioElement.value.addEventListener('play', () => { isPlaying.value = true; });
    audioElement.value.addEventListener('pause', () => { isPlaying.value = false; });
    audioElement.value.addEventListener('ended', () => { isPlaying.value = false; });
    audioElement.value.addEventListener('timeupdate', updateProgress);
    audioElement.value.addEventListener('loadedmetadata', updateProgress);
  }
});

onUnmounted(() => {
  if (audioElement.value) {
    audioElement.value.removeEventListener('play', () => { isPlaying.value = true; });
    audioElement.value.removeEventListener('pause', () => { isPlaying.value = false; });
    audioElement.value.removeEventListener('ended', () => { isPlaying.value = false; });
    audioElement.value.removeEventListener('timeupdate', updateProgress);
    audioElement.value.removeEventListener('loadedmetadata', updateProgress);
  }
});
</script>

<style scoped>
.audio-player {
  width: 100%;
  margin-top: 5px;
}

.player-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.play-button {
  background-color: #42b983;
  color: white;
  border: none;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 12px;
}

.progress-container {
  flex: 1;
  height: 6px;
  background-color: #eee;
  border-radius: 3px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  background-color: #42b983;
  width: 0;
  transition: width 0.1s linear;
}

.time-display {
  font-size: 12px;
  color: #777;
  min-width: 80px;
  text-align: right;
}
</style>
