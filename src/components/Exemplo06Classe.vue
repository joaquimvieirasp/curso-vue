<!-- SCRIPT -->
<script setup>
    let validacao = true;
</script>

<!-- CSS -->
<style>
    .ok{color: green;}

    .falha{color: red;}
</style>

<!-- HTML -->
<template>
    <p :class="validacao ? 'ok' : 'falha'">Utilizando a diretiva class</p>
</template>