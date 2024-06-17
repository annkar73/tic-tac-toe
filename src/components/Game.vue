<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import Welcome from './Welcome.vue';
import PlayerForm from './PlayerForm.vue';
import Rules from './Rules.vue';
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

const setPlayerName = (name: string) => {
    if (currentPlayer.value === 'X') {
        players.value.X = name;
        currentPlayer.value = 'O';
    } else {
        players.value.O = name;
        playersSet.value = true;
    }
};

const updateStats = (winner: string) => {
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
<div class="container">


    <!-- Visa PlayerForm.vue en gång för att lägga till spelare -->
    <div>
        <Welcome v-if="!playersSet" />
        <PlayerForm @set-player-name="setPlayerName" v-if="!playersSet" />
    <!-- Aktiva spelare -->
    <div v-if="playersSet">
        <h2>Tre-i-rad</h2>
    <div class="active">
        <h4>Aktiva spelare:</h4>
        <ul>
            <li v-for="(name, symbol) in players" :key="symbol"><strong>{{ symbol }}:</strong> {{ name }}</li>
        </ul>
    </div>
    </div>

        <Rules v-if="!playersSet" />
        <div class="board-container" v-if="playersSet">
            <TicTacToeBoard :players="players"  @game-over="updateStats" />
        </div>

        <!-- Knapp för att visa statistik -->
        <button @click="toggleStats" v-if="playersSet">Visa resultat</button>

        <div class="btn-container">
        <!-- Statistik över vinnare -->
        <div v-if="showStats && gamesPlayed > 0" >
            <WinnerStats v-if="showStats && gamesPlayed > 0" :winsX="winsX" :winsO="winsO" :gamesPlayed="gamesPlayed" />
        </div>
        <!-- Knapp för att återställa spelet -->
         <button @click="resetGame" v-if="playersSet">Börja från början</button>
        </div>
    </div>
</div>
</template>

<style scoped>
.active {
    color: #660e60;
}
ul {
    list-style-type: none;
}
.btn-container {
    display: flex;
    flex-direction: column;
}

button {
    margin: 10px 10px 5px 0;
    padding: 10px 10px;
    font-size: 16px;
    font-weight: 600;
    width: 320px;
    background-color: #ffa600;
    color: #660e60;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s ease;
    outline: none;
}
button:hover,
button:focus {
    background-color: #ff6361;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);

}
h2 {
    font-family: "Rock Salt", cursive;
    font-size: 24px;
    color: #660e60;
    margin-bottom: 10px;
}
h4 {
    font-size: 18px;
    margin-bottom: 5px;
    color: #660e60;
}
.container {
  width: 380px;
  margin: 0;
}

.board-container {
  margin-top: 20px;
}

</style>