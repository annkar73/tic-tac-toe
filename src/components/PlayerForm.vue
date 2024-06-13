<script setup lang="ts">
import { ref, defineEmits } from 'vue';

const playerX = ref('');
const playerO = ref('');

const emit = defineEmits(['set-names']);

const submitNames = () => {
    const names = { X: playerX.value, O: playerO.value };
    localStorage.setItem('playerNames', JSON.stringify(names));
    emit('set-names', names);
};

</script>

<template>
    <div>
        <form @submit.prevent="submitNames">
            <input v-model="playerX" placeholder="Spelare X" required>
            <input v-model="playerO" placeholder="Spelare O" required>
            <button type="submit">Starta spelet</button>
        </form>
    </div>

</template>

<style scoped>
</style>