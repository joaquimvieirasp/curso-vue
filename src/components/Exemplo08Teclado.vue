<!-- SCRIPT -->
<script setup>
    //Função de mensagem
    function mensagem(){
        alert('Executando a função');
    }
</script>

<!-- HTML -->
<template>
    <input type="text" @keyup.enter="mensagem"> 
</template>