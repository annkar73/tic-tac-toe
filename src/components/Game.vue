  <script setup lang="ts">
  import { ref, onMounted, watch } from 'vue';
  import PlayerForm from './PlayerForm.vue';
  import TicTacToeBoard from './TicTacToeBoard.vue';

  const players = ref({
    X: '',
    O: ''
  });

  const currentPlayer = ref('X'); // Start by adding player X
  const playersSet = ref(false);
  
  const setPlayerName = (name) => {
    if (currentPlayer.value === 'X') {
        players.value.X = name;
        currentPlayer.value = 'O';
    } else {
        players.value.O = name;
        playersSet.value = true;
    }
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
  });

  watch(players, (newPlayers) => {
    localStorage.setItem('players', JSON.stringify(newPlayers));
  }, {deep: true});
  </script>

  <template>

    <!-- Visa PlayerForm.vue en gång för att lägga till spelare -->
    <div>
      <PlayerForm @set-player-name="setPlayerName" v-if="!playersSet" />
      <TicTacToeBoard :players="players" v-if="playersSet"/>
    </div>
  </template>

  