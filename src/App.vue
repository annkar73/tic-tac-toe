<script setup lang="ts">
import { ref, onMounted } from 'vue';
import PlayerForm from '../src/components/PlayerForm.vue';

const playerNames = ref({ X: '', O: '' });

const setPlayerNames = (names) => {
  playerNames.value = names;
};

onMounted(() => {
  const savedNames = JSON.parse(localStorage.getItem('playerNames'));
  if (savedNames) {
    playerNames.value = savedNames;
  }
});

</script>

<template>
  <div id="app">
    <PlayerForm @set-names="setPlayerNames" />
    <div v-if="playerNames.X && playerNames.O">
      <p>Player X: {{ playerNames.X }}</p>
      <p>Player O: {{ playerNames.O }}</p>

    </div>
  </div>
</template>

<style scoped>

</style>
