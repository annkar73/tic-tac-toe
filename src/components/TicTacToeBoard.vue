<script setup lang="ts">
import { ref, defineProps, defineEmits } from 'vue';

const props = defineProps({
  players: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['game-over']);

const board = ref(Array(9).fill('')); 
const currentPlayer = ref('X'); 
const winner = ref(''); 

// Gör ett drag på spelbrädet
const makeMove = (index) => {
  if (board.value[index] === '' && !winner.value) {
    board.value[index] = currentPlayer.value; // Sätt aktuell spelares markering på vald cell

    if (checkWinner()) {
      winner.value = currentPlayer.value; // sätt vinnaren till aktuell spelare
      emit('game-over', winner.value);
    } else if (board.value.every(cell => cell !== '')) {
      winner.value = 'Oavgjort';
      emit('game-over', 'draw');
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
    <div v-if="winner !== undefined && winner.value != ''">
      <p v-if="winner !== 'Oavgjort'"><strong>Vinnare:</strong> {{ props.players[winner] }} ({{ winner }})</p>
      <p v-else>Oavgjort</p>

      <div class="game-over-sign" v-if="winner">
        <svg enable-background="new 0 0 24 24" id="Layer_1" version="1.1" viewBox="0 0 24 24" xml:space="preserve" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g><path d="M22,20.5H2   c-0.8284271,0-1.5-0.6715736-1.5-1.5V5c0-0.8284273,0.6715729-1.5,1.5-1.5h20c0.8284264,0,1.5,0.6715727,1.5,1.5v14   C23.5,19.8284264,22.8284264,20.5,22,20.5z" fill="none" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><path d="   M6,7.4999981c0-0.5522842-0.4477148-0.999999-0.999999-0.999999H4.999999C4.4477148,6.499999,4,6.9477139,4,7.4999981V9.5   c0,0.5522842,0.4477148,0.999999,0.999999,0.999999H5.000001C5.5522852,10.499999,6,10.0522842,6,9.5V8.999999H5.5" fill="none" id="XMLID_1376_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><g><path d="    M8,10.5V7.499999C8,6.9477148,8.4477148,6.5,8.999999,6.5H9.000001C9.5522852,6.5,10,6.9477148,10,7.499999V10.5" fill="none" id="XMLID_1331_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><line fill="none" id="XMLID_1330_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10" x1="8" x2="10" y1="9.5" y2="9.5"/></g><polyline fill="none" id="XMLID_1335_" points="   20,6.5 18,6.5 18,10.5 20,10.5  " stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><line fill="none" id="XMLID_1334_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10" x1="18" x2="19.5" y1="8.5" y2="8.5"/><polyline fill="none" id="XMLID_1302_" points="   15,13.5 13,13.5 13,17.5 15,17.5  " stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><line fill="none" id="XMLID_21_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10" x1="13" x2="14.5" y1="15.5" y2="15.5"/><path d="   M16,10.5V7.499999C16,6.9477148,15.5522852,6.5,15.000001,6.5H14.999999C14.4477148,6.5,14,6.9477148,14,7.499999V10.5V7.499999   C14,6.9477148,13.5522852,6.5,13.000001,6.5H12.999999C12.4477148,6.5,12,6.9477148,12,7.499999V10.5" fill="none" id="XMLID_1337_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><path d="   M5.999999,17.5H6.000001C6.5522852,17.5,7,17.0522861,7,16.5000019V14.499999C7,13.9477148,6.5522852,13.5,6.000001,13.5H5.999999   C5.4477148,13.5,5,13.9477148,5,14.499999v2.0000029C5,17.0522861,5.4477148,17.5,5.999999,17.5z" fill="none" id="XMLID_1346_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><path d="   M17,17.5v-4h1.0000019C18.5522861,13.5,19,13.9477148,19,14.499999v0.0000019C19,15.0522852,18.5522861,15.5,18.0000019,15.5H17h1   c0.5522842,0,1,0.4477148,1,1v1" fill="none" id="XMLID_1362_" stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/><polyline fill="none" id="XMLID_1357_" points="   9,13.5 9,16 10,17.5 11,16 11,13.5  " stroke="#303C42" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10"/></g><g/><g/><g/><g/><g/><g/><g/><g/><g/><g/><g/><g/><g/><g/><g/></svg>
      </div>
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
  background-color: #bc5090;
  color: antiquewhite;
  border: 1px solid darkgreen;
  font-family: "Rock Salt", cursive;
  font-size: 50px;
  font-weight: 700;
  cursor: pointer;
  transition: box-shadow 0.3s ease;
}
.cell:hover {
  background-color: #660e60;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border: 2px solid darkslategray;
}
button {
    margin: 10px 10px 10px 0;
    padding: 10px 20px;
    font-size: 16px;
    font-weight: 500;
    width: 320px;
    background-color: #ffa600;
    color: black;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s ease;
}
button:hover {
    background-color: #ff6361;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);

}

.game-over-sign {
  animation: fadeInScale 2s ease-in-out;
  margin: 5px auto;
}
@keyframes fadeInScale {
  from {
    opacity: 0;
    transform: scale(0.1);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}


</style>
