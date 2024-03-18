<template>
  <div id="allPage">
    <Navbar :logo="logoSource" :alt="altName" />
    <div>
      <RouterView />
    </div>
    <Footer />
  </div>
</template>

<script>
  import Navbar from "./components/Navbar.vue";
  import Footer from "./components/Footer.vue";
  import { RouterLink, RouterView } from 'vue-router'
  
  export default {
    components: {
      Navbar, 
      Footer
    },
    data() {
      return {
        logoSource: "../public/img/logo.png",
        altName: "Make Your Burger"
      }
    }
  }
</script>

<style>
  * {
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  #allPage {
    margin-bottom: 150px;
  }

  .mainContainer {
    margin: 50px;
    min-height: 250px;
  }

  h1 {
    text-align: center;
    font-size: 42px;
    margin-bottom: 30px;
    color: #222;
  }
</style>