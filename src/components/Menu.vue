<template>
    <div class="menu">
        <h3>{{ menu.strMeal }}</h3>
        <img :src="menu.strMealThumb" alt="menu-image">
        <button @click="fetchMeal" class="menu__recipe-button">Get Recipe</button>
        <p>{{ mealInstroction }}</p>
    </div>
</template>

<script>
export default {
    props: {
        menu: { type: Object },
    },

    data() {
        return {
            mealInstroction: '',
        }
        
    },

    methods: {
        async fetchMeal() {
            const ingredienceUrl = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.menu.idMeal}`;

            try {
                const responseDescription = await fetch(ingredienceUrl);
                const {meals} = await responseDescription.json();
                this.mealInstroction = (meals[0].strInstructions);

            } catch(error) {
                return 'error';
            }
        },
    }
}
</script>

<style>
    .menu {
        position: relative;
        /* display: flex; */
        /* width: 100%;
        height: 100%; */
        /* background-repeat: none; 
        background-position: center;
        background-size: cover; */
    }

    .menu h3 {
        position: absolute;
        /* background-color: var(--background); */
    }
</style>