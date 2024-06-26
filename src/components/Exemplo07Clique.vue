<!-- SCRIPT -->
<script setup>
    // funçao
    function mensagem(){
        alert('Trabalhando com evento de clique');
    }
</script>

<!-- HTML -->
<template>
    <button @click="mensagem">Clique aqui</button>
</template>