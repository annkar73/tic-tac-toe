<script setup lang="ts">
import { ref, defineProps } from 'vue';

const props = defineProps({
  players: {
    type: Object,
    required: true
  }
});

const board = ref(Array(9).fill('')); 
const currentPlayer = ref('X'); 
const winner = ref(''); 

// Gör ett drag på spelbrädet
const makeMove = (index) => {
  if (board.value[index] === '' && !winner.value) {
    board.value[index] = currentPlayer.value; // Sätt aktuell spelares markering på vald cell

    if (checkWinner()) {
      winner.value = currentPlayer.value; // sätt vinnaren till aktuell spelare
    } else if (board.value.every(cell => cell !== '')) {
      winner.value = 'Oavgjort';
    } else {
      currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'; // byt aktuell spelare
    }
  }
};

// Kontrollera om vinnare finns baserat på spelbrädets status
const checkWinner = () => {
  // Definiera alla möjliga vinstrader på spelbrädet
  const winningCombinations = [
    [0, 1, 2], // horisontella rader
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6], // vertikala rader
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8], // diagonala rader
    [2, 4, 6],
  ];
  // Loopa igenom alla vinstrader och kontrollera om någon är fylld av samma spelare
  return winningCombinations.some(combination => {
    const [a, b, c] = combination;
    return (
      board.value[a] &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    );
  });
};

// Återställ spelet till ursprungligt tillstånd
const resetGame = () => {
  board.value = Array(9).fill(''); // Återställ spelbrädet med tomma strängar
  currentPlayer.value = 'X'; // Återställ aktuell spelare till 'X'
  winner.value = '';
};
</script>

<template>
  <div>
    <p v-if="props.players[currentPlayer]">Det är <strong>{{ props.players[currentPlayer] }}s</strong> tur att spela</p>

    <!-- Spelbräde -->
    <div class="board">
      <div v-for="(cell, index) in board" :key="index" class="cell" @click="makeMove(index)">
        {{ cell }}
      </div>
    </div>

    <!-- Visa vinnare och knapp för att starta om ifall spelet är över -->
    <div v-if="winner !== undefined && winner.vaue != ''">
      <p v-if="winner !== 'Oavgjort'"><strong>Vinnare:</strong> {{ props.players[winner] }} ({{ winner }})</p>
      <p v-else>Oavgjort</p>
      <button @click="resetGame" class="resetBtn">Spela igen</button>
    </div>
  </div>
</template>

<style scoped>
.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 7px;
}
.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100px;
  height: 100px;
  background-color: olive;
  color: antiquewhite;
  border: 1px solid darkslategray;
  font-family: "Rock Salt", cursive;
  font-size: 50px;
  font-weight: 700;
  cursor: pointer;
  transition: box-shadow 0.3s ease;
}
.cell:hover {
  background-color: darkolivegreen;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border: 3px solid darkslategray;
}
.resetBtn {
    background-color: olive;
    color: antiquewhite;
    font-weight: bold;
}
.resetBtn:hover {
    background-color: darkolivegreen;
}
</style>
