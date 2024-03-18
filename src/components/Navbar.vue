<template>
    <div id="nav">
        <RouterLink to="/" id="logoUrl">
            <img :src="logo" id="logo" :alt="alt">
        </RouterLink>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/pedidos">Pedidos</RouterLink>
    </div>
</template>

<script>
export default {
    name: 'Navbar',
    props: ['logo', 'alt']
}
</script>

<style scoped>
#nav {
    background-color: #222;
    border-bottom: 4px solid #111;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    margin: 0;
}

#logoUrl {
    margin: auto;
    margin-left: 0;
}

#logoUrl img {
    max-width: 5rem;
}

#nav a {
    color: #FCBA03;
    text-decoration: none;
    margin-left: 50px;
    font-size: 20px;
    transition: .3s;
}

#nav a:hover {
    color: rgb(194, 158, 0);
}
</style>