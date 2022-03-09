<template>
    <div class="menu">
        <h5>{{ menu.error }}</h5>
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
        error: { type: String }
    },

    data() {
        return {
            mealRecipe: '',
            meal: [],
            // error: '',
        }
        
    },

    methods: {
        async fetchMeal() {
            const ingredienceUrl = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.menu.idMeal}`;
            const responseDescription = await fetch(ingredienceUrl);

            try {
                await this.handleResponse(responseDescription);
            } catch(error) {
                this.error = error.message;
            }
        },

        async handleResponse(responseDescription) {
            if(responseDescription.status >= 200 && responseDescription.status < 300) {
                const {meals} = await responseDescription.json();
                console.log(meals);
                this.meal = meals;
                // Calling the emit function and sending the recipe-Instruction in paramenter to emit 
                this.emitRecipe(this.meal);         
            } else {
                if(responseDescription.status === 404) {
                    throw new Error('Sorry, Weather could not be found. Please write the correct place.');
                }
                if(responseDescription.status === 401) {
                    throw new Error('Unauthorized');
                }
                if(responseDescription.status > 500) {
                    throw new Error('Servor Error!');
                }
                throw new Error('Something went wrong!');
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