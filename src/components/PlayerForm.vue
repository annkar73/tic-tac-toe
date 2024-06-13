<script setup lang="ts">
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['set-player-name']);
const playerName = ref('');

const submitPlayerName = () => {
  if (playerName.value.trim()) {
    emit('set-player-name', playerName.value);
    playerName.value = '';
  }
};
</script>

<template>
  <div>
    <h2>Lägg till spelare</h2>
    <input v-model="playerName" @keyup.enter="submitPlayerName" placeholder="Ange spelarnamn" />
    <button @click="submitPlayerName">Lägg till spelare</button>
  </div>
</template>
