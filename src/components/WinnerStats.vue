<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    winsX: Number,
    winsO: Number,
    gamesPlayed: Number
});


</script>

<template>
    <div class="stats-container">
    <!-- Vinnarstatistik -->
     <h2 class="stats-title">Spelstatistik</h2>
     <p><strong>Spelare X:</strong> {{ winsX }} vinster</p>
     <p><strong>Spelare O:</strong> {{ winsO }} vinster</p>
     <p><strong>Antal spel:</strong> {{ gamesPlayed }}</p>
</div>

</template>

<style scoped>
.stats-container {
    margin-top: 10px;
    padding: 10px;
}
.stats-title {
    font-size: 20px;
    margin-bottom: 10px;
    color: #660e60;
}

</style>