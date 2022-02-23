<template>
    <section class="home">
        <!-- <header class="home__header"> -->
            <Header />
        <!-- </header> -->

        <!-- <main class="home__main"> -->
            <Main />
        <!-- </main> -->
            
        <!-- <footer class="home__footer"> -->
            <Footer />
        <!-- </footer>  -->
    </section>  
</template>

<script>
import Header from './Header.vue';
import Main from './Main.vue';
import Footer from './Footer.vue';
    export default {
        components: {
            Header,
            Main,
            Footer,
        },

        data() {
            
        },
    }
</script>

<style>
    .home {
        width: 100vw;
        height: 100vh;
        /* display: grid;
        grid-template-rows: 100px 1fr 100px; */
        row-gap: 0;
    }

    .home__header {

    }
    
</style>

