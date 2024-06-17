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
    <input v-model="playerName" @keyup.enter="submitPlayerName" placeholder="Ange spelarnamn" />
    <button @click="submitPlayerName" class="add-btn">Lägg till spelare</button>
  </div>
</template>

<style scoped>
input {
    font-size: 16px;
    height: 35px;
    width: 320px;
    margin-top: 5px;
    border: 2px solid #ffa600;
    border-radius: 4px;
}
input:focus {
  outline: none;
  border: 2px solid #ff6361;
  box-shadow: 0 0 5px 2px #ff6361;
}
.add-btn {
    margin-top: 10px;
    padding: 10px 20px;
    font-size: 16px;
    font-weight: 600;
    height: auto;
    width: 320px;
    background-color: #ffa600;
    color: #660e60;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s ease;
    outline: none;
}
.add-btn:hover,
.add-btn:focus {
    background-color: #ff6361;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);

}


</style>