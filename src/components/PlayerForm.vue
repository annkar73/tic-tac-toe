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

<style scoped>
input {
    font-size: 16px;
    height: 35px;
}
button {
    margin: 10px;
    padding: 10px 20px;
    font-size: 16px;
    font-weight: 500;
    height: auto;
    width: auto;
    background-color: #ffa600;
    color: black;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s ease;
}
button:hover {
    background-color: #ff6361;
}


</style>