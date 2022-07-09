<template>
    <div>
        <p>Componente de mensagem</p>
        <div>
            <form id="burger-form">
                <div class="input-container">
                    <label for="name">Nome do cliente:</label>
                    <input type="text" id="name" class="name" v-model="name" placeholder="Digite o seu nome"/>
                </div>
                <div class="input-container">
                    <label for="bread">Escolha o pão:</label>
                    <select name="bread" id="bread" v-model="bread">
                        <option value="">Esolha o seu pão</option>
                        <option value="integral">Integral</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meat">Escolha a carne do seu burger:</label>
                    <select name="meat" id="meat" v-model="meat">
                        <option value="">Esolha o tipo de carne</option>
                        <option value="maminha">Maminha</option>
                    </select>
                </div>
                <div id="optional-container" class="input-container">
                    <label id="optional-title" for="optional">Selecione os opicionais:</label>
                    <div class="checkbox-container">
                        <input type="checkbox" name="optional" v-model="optional" value="salame">
                        <span>Salame</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" value="Criar meu burger" class="submit-btn">
                </div>
            </form>
        </div>
    </div>
</template>
<script>
    export default {
        name: 'BuergerForm',
    }
</script>
<style scoped>
    #burger-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #optional-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #optional-title {
        width: 100%;
    }

    .checkbox-container {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span {
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        box-sizing: border-box;
        transition: 500ms;
    }

    .submit-btn:hover {
        color: #222;
        background-color: transparent;
    }
</style>