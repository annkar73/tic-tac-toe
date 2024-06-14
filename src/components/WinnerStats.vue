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
     <p>Spelare X: {{ winsX }} vinster</p>
     <p>Spelare O: {{ winsO }} vinster</p>
     <p>Antal spel: {{ gamesPlayed }}</p>

</template>

<style scoped>

</style>