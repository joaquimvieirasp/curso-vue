<!-- SCRIPT -->
<script setup>

    //Obter props
    const props = defineProps(['nome', 'idade']);
</script>

<!-- HTML -->
<template>
    <h1>Nome: {{ nome }}</h1>
    <h1>Idade: {{ idade }}</h1>
</template>