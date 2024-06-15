<script setup lang="ts">

const welcomeTitle = "Luffarschack";

const welcomeText1 = "Välkommen till mitt lilla spel - luffarschack, tre i rad eller tic-tac-toe vilket man nu föredrar att kalla det.";
const welcomeText2 = "Börja med att fylla i varsitt spelarnamn, ett i taget, nedan så startar spelet automatiskt.";
</script>

<template>
<div class="container">
    <h1>{{ welcomeTitle }}</h1>

    <div>
        <p>{{ welcomeText1 }}</p>
        <p>{{ welcomeText2 }}</p>
    </div>
</div>
</template>

<style scoped>
.container {
    width: 320px;
    margin: 10px 10px 10px 0;
}
</style>