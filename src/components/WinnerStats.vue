<script setup>
import { ref, watch, onMounted, defineProps } from 'vue';

const props = defineProps({
    winsX: Number,
    winsO: Number,
    gamesPlayed: Number
});

const winsX = ref(0);
const winsO = ref(0);
const gamesPlayed = ref(0);

// updateStats for winner (X vs O)
const updateStats = (winner) => {
    gamesPlayed.value++;
    if (winner === 'X') {
        winsX.value++;
    } else if (winner === 'O') {
        winsO.value++;
    }
}

// watch for changes in statistics
watch([winsX, winsO], ([newWinsX, newWinsO]) => {
    localStorage.setItem('winStats', JSON.stringify({ winsX: newWinsX, winsO: newWinsO, gamesPlayed: gamesPlayed.value }));
}, { deep: true });

// Fetch stats from local storage
onMounted(() => {
    const storedStats = localStorage.getItem('winStats');
    if (storedStats) {
        const parsedStats = JSON.parse(storedStats);
        winsX.value = parsedStats.winsX || 0;
        winsO.value = parsedStats.winsO || 0;
        gamesPlayed.value = parsedStats.gamesPlayed || 0;
    }
});

</script>

<template>
    <!-- Vinnarstatistik -->
     <h2>Vinnarstatistik</h2>
     <p>Spelare X: {{ props.winsX }} vinster</p>
     <p>Spelare O: {{ props.winsO }} vinster</p>
     <p>Antal spel: {{ props.gamesPlayed }}</p>

</template>

<style scoped>

</style>