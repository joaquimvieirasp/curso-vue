<!-- SCRIPT -->
<script setup>
    // Função para validar o formulário
    function validaFormulario(){
        alert('Formulário enviado');
    }
</script>

<!-- HTML -->
<template>
    <form @submit="validaFormulario">
        <input type="submit" value="Enviar formulário">
    </form>
</template>