<script setup lang="ts">

const rulesTitle = "Så här spelar man:";

const rulesText ="Spelarna tilldelas symbolen X eller O. ";
const rulesText2 = "Spelare X inleder spelet med att klicka i en ruta på spelplanen. Spelarna turas sedan om att markera rutor och målet är att  få ihop en rad med sin egen symbol. Raden kan vara vågrät, lodrät eller diagonal. Man måste även tänka på att försöka blockera motspelaren. När någon får ihop en rad vinner personen som fick ihop raden. ";
</script>

<template>
<div class="rules-container">
    <h3 class="title">{{ rulesTitle }}</h3>

    <div>
        <p class="rules-intro">{{ rulesText }}</p>
        <p>{{ rulesText2 }}</p>
    </div>
</div>
</template>

<style scoped>
.rules-container {
    width: 320px;

}
.title {
    font-size: 20px;
    margin-bottom: 15px;
}
.rules-intro {
    font-style: italic;
}
</style>