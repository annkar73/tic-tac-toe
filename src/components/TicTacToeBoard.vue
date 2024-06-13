<script setup lang="ts">
import { ref } from 'vue';

const board = ref(Array(9).fill('')); // Array med 9 tomma strängar för varje cell på spelbrädet
const currentPlayer = ref('X'); // Spelare aktuell för att göra ett drag ('X' eller 'O')
const winner = ref(''); // Håller vinnare av spelet när det finns en vinnare

// Gör ett drag på spelbrädet
const makeMove = (index: number) => {
    if (board.value[index] === '' && !winner.value) {
        board.value[index] = currentPlayer.value; // Sätt aktuell spelares markering på vald cell

        if (checkWinner()) {
            winner.value = currentPlayer.value; // sätt  vinnaren till aktuell spelare
        } else {
            currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'; // byt aktuell spelare
        }
    }
};
// kontrollera om vinnare finns baserat på spelbrädets status
const checkWinner = () => {
    // definiera alla möjliga vinstrader på spelbrädet
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
    // loopa igenom alla vinstrader och kontrollera om någon är fylld av samma spelare
    return winningCombinations.some(combination => {
        const [a, b, c] = combination;
        return (
            board.value[a] &&
            board.value[a] === board.value[b] &&
            board.value[a] === board.value[c]
        );
    });
};

// reset game to original state
const resetGame = () => {
    board.value = Array(9).fill(''); // Återställ spelbrädet med tomma strängar
    currentPlayer.value = 'X'; // Återställ aktuell spelare till 'X'
    winner.value = '';
};

</script>

<template>
    <!-- build a game board-->
     <div class="board">
        <div v-for="(cell, index) in board" :key="index" class="cell" @click="makeMove(index)">
            {{ cell }}
        </div>

     </div>
     <!--show winner and button to restart if game over -->
     <div v-if="winner">
        <p>Winner: {{ winner }}</p>
        <button @click="resetGame">Spela igen</button>

     </div>

</template>

<style scoped>

.board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 5px;
}
.cell {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100px;
    height: 100px;
    background-color: aquamarine;
    font-size: 24px;
    cursor: pointer;
}
.cell:hover {
    background-color: turquoise;
}

</style>