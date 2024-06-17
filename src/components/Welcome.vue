<script setup lang="ts">

const welcomeTitle = "Tre-i-rad";

const welcomeText1 = "Välkommen till mitt lilla spel.";
const welcomeText2 = "Börja med att fylla i varsitt spelarnamn nedan så startar spelet automatiskt.";
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
h1 {
    font-family: "Rock Salt", cursive;
}
.container {
    max-width: 320px;
    margin-left: 0;
    color: #660e60;
}
</style>