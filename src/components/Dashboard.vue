<!-- 
    npm create vue@latest 
    npm install
    npm install json-server
    npm run serve
    npm run backend
-->

<template>
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
        <div id="burgerTableRows" v-for="pedido in burgers" >
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
                    <select name="status" class="status">
                        <option value="">Selecione</option>
                    </select>
                    <button class="deleteBtn">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burgersId: null,
            status: []
        }
    },
    methods: {
        async getPedidos() {
            const req = await fetch('http://localhost:3000/burgers')
            const data = await req.json()
            this.burgers = data;
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