<template>
    <div class="menu">
        <h5>{{ menu.error }}</h5>
        <h3>{{ menu.strMeal }}</h3>
        <img :src="menu.strMealThumb" alt="menu-image">
        <button @click="fetchMeal(); emitToggleRecipe(); emitRecipeIntro();" class="menu__recipe-button">Get Recipe</button>
    </div>
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
            recipeError: '',
        }
        
    },

    methods: {
        async fetchMeal() {
            const ingredienceUrl = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.menu.idMeal}`;
            const responseDescription = await fetch(ingredienceUrl);

            try {
                await this.handleResponse(responseDescription);

            } catch(error) {
                this.recipeError = error.message;
                this.emitRecipeError(this.recipeError);
            }
        },

        async handleResponse(responseDescription) {
            if(responseDescription.status >= 200 && responseDescription.status < 300) {
                const { meals } = await responseDescription.json();
                
                this.meal = meals;
                // Calling the emit function and sending the recipe-Instruction in paramenter to emit 
                this.emitRecipeIntro(this.meal);         
            } else {
                if(responseDescription.status === 404) {
                    throw new Error('Sorry, Page could not be found');
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

        // Emit recipe instruction to parent componet with meal array
        emitRecipeIntro() {
            this.$emit('get-recipe-instruction', this.meal);
        },

        emitToggleRecipe() {
            this.$emit('toggle-recipe'); 
        },

        // Emit error message to the parent component with error
        emitRecipeError(error) {
            this.$emit('get-recipe-error', error);
        }
    }
}
</script>

<style>
    .menu {
        position: relative;
        bottom: var(--bottom-medium);
    }

    .menu button {
        position: absolute;
        top: 75%;
        left: 25%;
        display: flex;
        padding: var(--padding-small);
        border-radius: 20px;
        align-content: center;
        background-color: var(--header);
        color: var(--background);
    }

    /* Medium screen devices (968px and above) */
    @media screen and (min-width: 968px) {
        .menu button {
            top: 85%;
            left: 25%;
        }
    }
</style>