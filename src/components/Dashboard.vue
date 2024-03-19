<!-- 
    npm create vue@latest 
    npm install
    npm install json-server
    npm run serve
    npm run backend
-->

<template>
    <Mensagem :msg="message" v-show="message" :classe="classe"/>
    <div id="burgerTable">
        <div>
            <div id="burgerTableHeading">
                <div class="orderId">#</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burgerTableRows" v-for="pedido in burgers" :key="pedido.id">
            <div class="burgerTableRow">
                <div class="orderNumber">{{ pedido.id }}</div>
                <div>{{ pedido.nome }}</div>
                <div>{{ pedido.pao }}</div>
                <div>{{ pedido.carne }}</div>
                <div>
                    <ul v-for="(opcional, index) in pedido.opcionais" :key="index">
                        <li >{{ opcional }}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateStatus($event, pedido.id)">
                        <option value="">Selecione</option>
                        <option v-for="statusItem in status" :value="statusItem.tipo" :key="statusItem.id" :selected="pedido.status == statusItem.tipo">
                            {{ statusItem.tipo }}
                        </option>
                    </select>
                    <button class="deleteBtn" @click="cancelarPedido(pedido.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Mensagem from "./Mensagem.vue"

export default {
    name: "Dashboard",
    components: {
        Mensagem
    },
    data() {
        return {
            burgers: null,
            burgersId: null,
            status: [],
            message: null,
            classe: ''
        }
    },
    methods: {
        async getPedidos() {
            const req = await fetch('http://localhost:3000/burgers')
            const data = await req.json()
            this.burgers = data;
            const statusData = this.getStatus()
        },
        async getStatus() {
            const req = await fetch('http://localhost:3000/status');
            const dataStatus = await req.json();
            this.status = dataStatus;
        },
        async cancelarPedido(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE",
                'Content-Type': 'application/json'
            })
            this.classe = 'red'
            this.message = "Pedido removido com sucesso!"
            setTimeout(() => this.message = '', 3000)

            this.getPedidos()
        },
        async updateStatus(event, id) {
            const opcao = event.target.value
            const dataJson = JSON.stringify({status: opcao})
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: {
                    'Content-Type': 'application/json'
                },
                body: dataJson
            });
            const res = await req.json();

            this.classe = 'blue'
            this.message = ` O Pedido com id ${id} foi atualizado para ${opcao}!`
            setTimeout(() => this.message = '', 3000)

            this.getPedidos()
        }
    },
    mounted() {
        this.getPedidos();
    }

}
</script>

<style scoped>
    #burgerTable {
        max-width: 1200px;
        margin: 0 auto;
    }
    #burgerTableHeading,
    #burgerTableRows,
    .burgerTableRow {
        display: flex;
        flex-wrap: wrap;
    }

    #burgerTableHeading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #burgerTableHeading div,
    .burgerTableRow div {
        width: 19%;
    }

    .burgerTableRow {
        width: 100%;
        padding: 12px;
        border: 1px solid #ccc;
    }

    #burgerTableHeading .orderId,
    .burgerTableRow .orderNumber {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .deleteBtn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .deleteBtn:hover{
        background-color: transparent;
        color: #222;
    }
</style>