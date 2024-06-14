<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import PlayerForm from './PlayerForm.vue';
import TicTacToeBoard from './TicTacToeBoard.vue';
import WinnerStats from './WinnerStats.vue';

const players = ref({
    X: '',
    O: ''
});

const currentPlayer = ref('X'); // Start by adding player X
const playersSet = ref(false);
const gamesPlayed = ref(0);
const winsX = ref(0); // Antal vinster för spelare X
const winsO = ref(0); // Antal vinster för spelare O
const showStats = ref(false); 

const setPlayerName = (name) => {
    if (currentPlayer.value === 'X') {
        players.value.X = name;
        currentPlayer.value = 'O';
    } else {
        players.value.O = name;
        playersSet.value = true;
    }
};

const updateStats = (winner) => {
    console.log('updateStats called for winner:', winner);
    
    gamesPlayed.value++;
    if (winner === 'X') {
        winsX.value++;
    } else if (winner === 'O') {
        winsO.value++;
    }
    saveStatsToLocalStorage();
};
const saveStatsToLocalStorage = () => {
    const stats = {
        gamesPlayed: gamesPlayed.value,
        winsX: winsX.value,
        winsO: winsO.value
    };
    console.log('saveStatsToLocalStorage called');
    
    localStorage.setItem('gameStats', JSON.stringify(stats));
};

const resetGame = () => {
    players.value = { X: '', O: ''};
    currentPlayer.value = 'X';
    playersSet.value = false;
    gamesPlayed.value = 0;
    winsX.value = 0;
    winsO.value = 0;
    localStorage.removeItem('players');
    localStorage.removeItem('gameStats');
};

const toggleStats = () => {
    showStats.value = !showStats.value;
    console.log('toggleStats called, showStats', showStats.value);
    
};

onMounted(() => {
    const storedPlayers = localStorage.getItem('players');
    if (storedPlayers) {
        const parsedPlayers = JSON.parse(storedPlayers);
        players.value.X = parsedPlayers.X || '';
        players.value.O = parsedPlayers.O || '';
        if (players.value.X && players.value.O) {
            playersSet.value = true;
        }
    }
    
    const storedStats = localStorage.getItem('gameStats');
    if (storedStats) {
        const parsedStats = JSON.parse(storedStats);
        gamesPlayed.value = parsedStats.gamesPlayed || 0;
        winsX.value = parsedStats.winsX || 0;
        winsO.value = parsedStats.winsO || 0;
    }
});


watch(players, (newPlayers) => {
    localStorage.setItem('players', JSON.stringify(newPlayers));
}, { deep: true });

watch([gamesPlayed, winsX, winsO], ([newGamesPlayed, newWinsX, newWinsO]) => {
    console.log('Stats changed:', { newGamesPlayed, newWinsX, newWinsO });
    
    saveStatsToLocalStorage();
}, { deep: true });

</script>

<template>
    <!-- Aktiva spelare -->
    <div v-if="playersSet">
        <h2>Luffarschack</h2>

        <h4>Aktiva spelare:</h4>
        <ul class="active">
            <li v-for="(name, symbol) in players" :key="symbol"><strong>{{ symbol }}:</strong> {{ name }}</li>
        </ul>

    </div>


    <!-- Visa PlayerForm.vue en gång för att lägga till spelare -->
    <div>
        <PlayerForm @set-player-name="setPlayerName" v-if="!playersSet" />
        <TicTacToeBoard :players="players" v-if="playersSet" @game-over="updateStats" />

        <!-- Knapp för att visa statistik -->
        <button @click="toggleStats">Visa vinnarstatistik</button>
        <!-- Statistik över vinnare -->
        <div v-if="showStats && gamesPlayed > 0">
            <WinnerStats :winsX="winsX" :winsO="winsO" :gamesPlayed="gamesPlayed" />
        </div>
        <!-- Knapp för att återställa spelet -->
         <button @click="resetGame">Börja från början</button>

    </div>
</template>

<style scoped>
.active {
    list-style-type: none;
}
button {
    margin: 10px;
}
</style>