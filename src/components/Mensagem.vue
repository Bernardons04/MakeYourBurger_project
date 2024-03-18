<template>
    <div class="divMensagem">
        <p>{{ msg }}</p>
    </div>
</template>

<script>
export default {
    name: "Mensagem",
    props: {
        msg: String
    }
}
</script>

<style>
    .divMensagem {
        background-color: #73aafc;
        border: solid 2px #0047b1;
        padding: 10px 15px;
        width: 400px;
        margin: 0 auto;
        margin-bottom: 20px;
        border-radius: 10px;
    }
</style>