<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    winsX: Number,
    winsO: Number,
    gamesPlayed: Number
});


</script>

<template>
    <!-- Vinnarstatistik -->
     <h2>Vinnarstatistik</h2>
     <p><strong>Spelare X:</strong> {{ winsX }} vinster</p>
     <p><strong>Spelare O:</strong> {{ winsO }} vinster</p>
     <p><strong>Antal spel:</strong> {{ gamesPlayed }}</p>

</template>

<style scoped>

</style>