<template>
    <!-- <div class="home"> -->
        <Header />
        <Footer />
    <!-- </div> -->
    
</template>

<script>
import Header from './Header.vue';
import Footer from './Footer.vue';
    export default {
        components: {
            Header,
            Footer,

        },

        data() {
            
        },
    }
</script>

<style>
    .home {
        position: relative;
        /* width: 100%; */
        /* height: 100%; */
    }
</style>

