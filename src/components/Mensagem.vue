<template>
    <div id="divMensagem" :class="classe">
        <p>{{ msg }}</p>
    </div>
</template>

<script>
export default {
    name: "Mensagem",
    props: {
        msg: String,
        classe: String
    }
}
</script>

<style>
    #divMensagem {
        padding: 10px 15px;
        width: 400px;
        margin: 0 auto;
        margin-bottom: 20px;
        border-radius: 10px;
    }

    .green {
        background-color: #77fc73;
        border: solid 2px #29b100;
    }

    .blue {
        background-color: #73aafc;
        border: solid 2px #0047b1;
    }

    .red {
        background-color: #fc7373;
        border: solid 2px #b10000;
    }
</style>