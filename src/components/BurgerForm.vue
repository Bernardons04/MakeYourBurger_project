<template>
    <div>
        <Mensagem :msg="message" v-show="message" />
        <div>
            <form id="burgerFormId" @submit="createBurger">
                <div class="inputContainer">
                    <label for="nome">Nome do cliente:</label>
                    <input type="text" id="nome" name="name" v-model="nome" placeholder="Digite o seu nome">
                </div>

                <div class="inputContainer">
                    <label for="pao">Escolha o pão:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Seleciona o seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                    </select>
                </div>

                <div class="inputContainer">
                    <label for="carne">Escolha a carne:</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Seleciona o tipo de carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
                    </select>
                </div>

                <div id="opcionaisContainer" class="inputContainer">
                    <label id="opcionaisTitle" for="opcionais">Selecione os opcionais:</label>
                    <div v-for="opcao in opcionaisData" class="checkboxContainer">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :key="opcao.id" :value="opcao.tipo">
                        <span>{{ opcao.tipo }}</span>
                    </div>
                </div>

                <div class="inputContainer">
                    <input type="submit" class="submitBtn" value="Criar meu Burger!">
                </div>
            </form>
        </div>
    </div>
</template>


<script>
import Mensagem from "./Mensagem.vue"

export default {
    name: "BurgerForm",
    components: {
        Mensagem
    },
    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisData: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            message: null,
        }
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes")
            const data = await req.json();

            this.paes = data.paes
            this.carnes = data.carnes
            this.opcionaisData = data.opcionais
        },
        async createBurger(e) {
            e.preventDefault()
            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado",
            }

            const req = await fetch('http://localhost:3000/burgers', {
                method: "POST",
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            })
            const res = await req.json();
            console.log(res)
            this.handleMessage()
            this.dataNull()
        },
        handleMessage() {
            this.message = "Pedido realizado com sucesso!"
            setTimeout(() => this.message = "", 3000);
        },
        dataNull() {
            this.nome = ''
            this.carne = ''
            this.pao = ''
            this.opcionais = ''
        }
    },
    mounted() {
        this.getIngredientes()
    }
}
</script>

<style scoped>
    #burgerFormId {
        max-width: 400px;
        margin: 0 auto;
    }

    .inputContainer {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    .inputContainer input,
    .inputContainer select {
        padding: 15px 10px;
    }

    label {
        font-weight: bold;
        margin-bottom: 5px;
        color: #333;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input,
    select {
        padding: 5px, 10px;
        width: 300px;
    }

    #opcionaisContainer {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #opcionaisTitle {
        width: 100%;
    }

    .checkboxContainer {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkboxContainer span,
    .checkboxContainer input {
        width: auto;
    }

    .checkboxContainer span {
        margin-left: 6px;
        font-weight: bold;
    }

    .submitBtn {
         
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        transition: .5s;
        cursor: pointer;
    }

    .submitBtn:hover {
        background-color: #222;
        color: #FCBA03;
    }
</style>