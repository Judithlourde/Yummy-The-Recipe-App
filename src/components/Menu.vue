<template>
    <div class="menu">
        <h3>{{ menu.strMeal }}</h3>
        <img :src="menu.strMealThumb" alt="menu-image">
        <button @click="fetchMeal(); emitRecipe(); emitRecipeIntro();" class="menu__recipe-button">Get Recipe</button>
    </div>

    <!-- <div class="menu-instruction">
        <p>{{ mealInstroction }}</p>
    </div> -->
</template>

<script>
export default {
    props: {
        menu: { type: Object },
    },

    data() {
        return {
            mealRecipe: '',
            meal: [],
        }
        
    },

    methods: {
        async fetchMeal() {
            const ingredienceUrl = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.menu.idMeal}`;

            try {
                const responseDescription = await fetch(ingredienceUrl);
                const {meals} = await responseDescription.json();
                console.log(meals);
                this.meal = meals;
                // this.mealRecipe = (meals[0].strInstructions);
                this.emitRecipe(this.meal);               // Calling the emit function and sending the recipe-Instruction in paramenter to emit 

            } catch(error) {
                return 'error';
            }
        },

        emitRecipe() {
            this.$emit('get-recipe-instruction', this.meal);
            console.log(this.meal);
        },

        emitRecipeIntro() {
            this.$emit('get-recipe', this.meal)
        }
    }
}
</script>

<style>
    .menu {
        position: relative;
    }

    .menu button {
        position: absolute;
        top: 88%;
        left: 30%;
        display: flex;
        padding: 10px;
        border-radius: 20px;
        align-content: center;
        background-color: var(--header);
    }
</style>