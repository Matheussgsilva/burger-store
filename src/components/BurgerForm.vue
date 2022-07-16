<template>
    <div>
        <Message :msg='msg' v-show='msg'/>
        <div>
            <form id="burger-form" @submit="createBurger">
                <div class="input-container">
                    <label for="name">Nome do cliente:</label>
                    <input type="text" id="name" class="name" v-model="name" placeholder="Digite o seu nome"/>
                </div>
                <div class="input-container">
                    <label for="bread">Escolha o pão:</label>
                    <select name="bread" id="bread" v-model="bread">
                        <option value="">Esolha o seu pão</option>
                        <option v-for="bread in breadData" :key="bread.id" :value="bread.tipo">{{ bread.tipo }}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meat">Escolha a carne do seu burger:</label>
                    <select name="meat" id="meat" v-model="meat">
                        <option value="">Esolha o tipo de carne</option>
                        <option v-for="meat in meatData" :key="meat.id" :value="meat.tipo">{{ meat.tipo }}</option>
                    </select>
                </div>
                <div id="optional-container" class="input-container">
                    <label id="optional-title" for="optionals">Selecione os opicionais:</label>
                    <div class="checkbox-container" v-for="optional in optionalData" :key="optional.id">
                        <input type="checkbox" name="optionals" v-model="optionals" :value="optional.tipo">
                        <span>{{ optional.tipo }}</span>
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
    import Message from './Message.vue'

    export default {
        name: 'BuergerForm',
        components: {
            Message
        },
        data() {
            return {
                breadData: null,
                meatData: null,
                optionalData: null,
                name: null,
                bread: null,
                meat: null,
                optionals: [],
                msg: null
            }
        },
        methods: {
            async getIngredients() {
                const req = await fetch('http://localhost:3000/ingredientes')
                const data = await req.json()

                this.breadData = data.paes
                this.meatData = data.carnes
                this.optionalData = data.opcionais
            },
            async createBurger(e) {
                e.preventDefault()

                const data = {
                    name: this.name,
                    meat: this.meat,
                    bread: this.bread,
                    optionals: Array.from(this.optionals),
                    status: 'Solicitado'
                }

                const dataJson = JSON.stringify(data)

                const req = await fetch('http://localhost:3000/burgers', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: dataJson
                })

                const res = await req.json()

                this.msg = `Pedido nº ${res.id} realizado com sucesso`

                setTimeout(() => this.msg = "", 3000)

                this.bread = []
                this.meat = []
                this.optionals = []
                this.name = []
            }
        },
        mounted() {
            this.getIngredients()
        }
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