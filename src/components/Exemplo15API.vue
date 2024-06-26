<!-- SCRIPT -->
<script setup>

    // Importar Bootstrap
    import 'bootstrap/dist/css/bootstrap.min.css'

    // Importação - VUE
    import { onMounted, ref } from "vue";

    // Vetor de produtos
    let produtos = ref([]);

    // Visibilidade dos botões
    let btnCadastrar = ref(true);

    // OnMounted
    onMounted(() => {
        fetch('http://localhost:3000/produtos')
        .then(requisicao => requisicao.json())
        .then(retorno => produtos.value = retorno);
    });

    // Objeto do tipo produto
    let obj = ref({ 'id': produtos.value.length + 1, 'produto': '', 'valor': 0 }); 

    // Função para cadastrar produtos
    function cadastrar(){
        
        if (produtos.value.length > 0) {
        obj.value.id = produtos.value[produtos.value.length - 1].id + 1;
    } else {
        obj.value.id = 1;
    }

        // Requisição
        fetch('http://localhost:3000/produtos', {
            method:'POST',
            body:JSON.stringify(obj.value),
            headers: {'Content-type':'application/json'}
        })
        .then(requisicao => requisicao.json())
        .then(retorno => {

            // Cadastrar o produto no vetor
            produtos.value.push(retorno)

            // Limpar os inputs
            obj.value.produto = '';
            obj.value.valor = 0;

        });
        
        // PreventDefault
        event.preventDefault();
    }

        // Função para selecionar um produto específico
        function selecionar(indice){
            obj.value = {
                id : produtos.value[indice].id,
                produto : produtos.value[indice].produto,
                valor : produtos.value[indice].valor
            }

            btnCadastrar.value = false;
        }

            // Função para editar produtos
    function editar(){
        
        // Requisição
        fetch('http://localhost:3000/produtos/'+obj.value.id, {
            method:'PUT',
            body:JSON.stringify(obj.value),
            headers: {'Content-type':'application/json'}
        })
        .then(requisicao => requisicao.json())
        .then(retorno => {

            // Obter o indice do vetor
            let indiceProduto = produtos.value.findIndex(objP => {
                return objP.id === retorno.id;
            });

            // Editar o produto no vetor
            produtos.value[indiceProduto] = retorno;

            // Alterar a visibilidade dos botões
            btnCadastrar.value = true;

            // Limpar os inputs
            obj.value.id = 0;
            obj.value.produto = '';
            obj.value.valor = 0;

        });
    }

       // Função para remover produtos
       function remover(){
        
        // Requisição
        fetch('http://localhost:3000/produtos/'+obj.value.id, {
            method:'DELETE',
            headers: {'Content-type':'application/json'}
        })
        .then(requisicao => requisicao.json())
        .then(() => {

            // Obter o indice do vetor
            let indiceProduto = produtos.value.findIndex(objP => {
                return objP.id === obj.value.id;
            });

            // Remover o produto no vetor
            produtos.value.splice(indiceProduto, 1);

            // Alterar a visibilidade dos botões
            btnCadastrar.value = true;

            // Limpar os inputs
            obj.value.id = 0;
            obj.value.produto = '';
            obj.value.valor = 0;

        });
    }

</script>

<!-- CSS -->
<style>
    form{
        width: 50%;
        margin: 30px auto;
        text-align: center;
    }

    input{
        margin-bottom: 10px;
    }

    .espacamentoBtn{
        margin-left: 5px;
        margin-right: 5px;

    }
</style>

<!-- HTML -->
<template>

    <!-- Formulário -->
    <form @submit="cadastrar">
        <input type="hidden" v-model="obj.id">
        <input type="text" placeholder="Produto" class="form-control" v-model="obj.produto">
        <input type="number" placeholder="Valor" class="form-control" v-model="obj.valor">
        <input type="submit" v-if="btnCadastrar" value="Cadastrar" class="btn btn-primary">
        <input type="button" @click="editar" v-if="!btnCadastrar" value="Editar" class="btn btn-primary espacamentoBtn"> 
        <input type="button" @click="remover" v-if="!btnCadastrar" value="Remover" class="btn btn-primary">
    </form>
    
    <!-- Tabela -->
    <table class="table table-striped">
        <thead>
            <tr>
                <th>Produto</th>
                <th>Valor</th>
                <th>Selecionar</th>
            </tr>
        </thead>

        <tbody>
            <tr v-for="(p, indice) in produtos">
                <td>{{ p.produto }}</td>
                <td>{{ p.valor }}</td>
                <td><button @click="selecionar(indice)" class="btn btn-primary">Selecioanar</button></td>
            </tr>
        </tbody>

    </table>
</template>