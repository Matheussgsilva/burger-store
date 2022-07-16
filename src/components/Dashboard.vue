<template>
    <div id="burger-table">
        <Message :msg='msg' v-show='msg'/>
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne</div>
                <div>Opicionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgerOrders" :key="burger.id">
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.name }}</div>
                <div>{{ burger.bread}}</div>
                <div>{{ burger.meat}}</div>
                <div>
                    <ul>
                        <li v-for="(optional, index) in burger.optionals" :key="index">
                            {{ optional }}
                        </li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateOrder($event, burger.id)">
                        <option v-for="orderStatus in status" :key="orderStatus.id" :value="orderStatus.tipo" :selected="burger.status === orderStatus.tipo">
                            {{ orderStatus.tipo }}
                        </option>
                    </select>
                    <button class="delete-btn" @click="deleteOrder(burger.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import Message from './Message.vue'

    export default {
        name: 'Dashboard',
        components: {
            Message
        },
        data() {
            return {
                burgerOrders: null,
                status: [],
                msg: null
            }
        },
        methods: {
            async getRequests() {
                const req = await fetch('http://localhost:3000/burgers')
                const data = await req.json()

                this.burgerOrders = data
            },
            async getStatus() {
                const req = await fetch('http://localhost:3000/status')
                const data = await req.json()

                this.status = data
            },
            async deleteOrder(id) {
                const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                    method: "DELETE"
                })
                const res = await req.json()

                this.msg = `Pedido nº ${id} removido com sucesso`

                setTimeout(() => this.msg = "", 3000)

                this.getRequests()
            },
            async updateOrder(event, id) {
                const option = event.target.value

                const dataJson = JSON.stringify({ status: option })

                const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                    method: "PATCH",
                    headers: { "Content-Type": "application/json" },
                    body: dataJson
                })

                const res = await req.json()

                this.msg = `Pedido nº ${res.id} foi atualizado para ${res.status}`

                setTimeout(() => this.msg = "", 3000)

                console.log(res.status)
            }
        },
        mounted() {
            this.getRequests(),
            this.getStatus()
        }
    }
</script>
<style scoped>
    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #burger-table-heading,
    #burger-table-rows,
    .burger-table-row {
        display: flex;
        flex-wrap: wrap;
    }

    #burger-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #burger-table-heading div,
    .burger-table-row div {
        width: 19%;
    }

    .burger-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }

    #burger-table-heading .order-id,
    .burger-table-row .order-number {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin: 12px;
    }

    .delete-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: 500ms;
    }

    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>