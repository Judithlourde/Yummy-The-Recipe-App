<template>
	<Home />
</template>

<script>
import Home from '../components/Home.vue';
	export default {
		components: {
            Home
        }, 
		data() {
			clientID: import.meta.env.VITE_APP_NAME
		}
	}
</script>


