<template>
    <Header />
  	<Main />
    <Footer />
</template>

<script>
import Header from '../components/Header.vue';
import Main from '../components/Main.vue';
import Footer from '../components/Footer.vue';
    export default {
        components: {
            Header,
            Main,
            Footer,
        },

        data() {
			clientID: import.meta.env.VITE_APP_NAME
		}
    }
</script>




