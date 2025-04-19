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


