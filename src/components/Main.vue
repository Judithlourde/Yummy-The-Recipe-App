<template>
    <main class="main">
        <div class="main__hero">
            <div class="main__hero-search">
                <input class="" type="text" placeholder="Search" v-model="mainIngredient">

                <button @click="fetchMenu">
                    <img src="/images/svg/search.svg" alt="search-icon">
                </button>
            </div> 
        </div>

        <div class="main__recipe-display">
            <h3>Dinner Ideas.....</h3>
            <p>Busy week? We can help!You can search and pick for delicious meal - including recipes.</p>
        </div>
    </main>
</template>

<script>
    export default {
        data() {
            return {
                mainIngredient: 'salmon',
                mealId: '',

            
            }
        },

        created() {
            this.fetchMenu();
        },

        methods: {
            async fetchMenu() {
                const recipeUrl = `https://www.themealdb.com/api/json/v1/1/filter.php?i=${this.mainIngredient}`;
                
                try {
                    const responseMenu = await fetch(recipeUrl);
                    const {meals} = await responseMenu.json();
                    // const result = meals;
                    console.log(meals);
                    this.mealId = meals[0].idMeal;
                    this.fetchMeal();

                } catch(error) {
                    return 'error';
                }
            },

            async fetchMeal() {
                const ingredienceUrl = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.mealId}`;

                try {
                    const responseDescription = await fetch(ingredienceUrl);
                    const {meals} = await responseDescription.json();
                    console.log(meals[0].strInstructions);

                } catch(error) {
                    return 'error';
                }
            }
        }
        
    }
</script>

<style>
    .main {
        display: grid;
        grid-template-columns: var(--grid-column-12);
        width: 100%;
        height: 100%;
    }

    .main__hero {
        grid-column: 1/ span 12;
        background-image: url('/images/fish_curry.jpg');
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center;
        position: relative;
    }

    .main__hero-search {
        display: flex;
        justify-content: space-between;
        position: absolute;
        top: 120px;
        width: 80%;
        padding: var(--padding-small);
        margin-left: 20px;
        background-color: var(--background);
        border-radius: 20px; 
    }

    .main__hero-search input {
        outline: none;
        border: none;
    }

    .main__recipe-display {
        grid-column: 1/ span 12;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        width: 300px;
        margin-left: auto;
        margin-right: auto;    
    }

    .main__recipe-display h3 {
        padding: var(--padding-medium) 0;
    }

    /* Medium screen devices (968px and above) */
    @media screen and (min-width: 968px) {
        .main {
            display: grid;
            grid-template-columns: var(--grid-column-12);
        }

        .main__hero {
            grid-column: 1/ span 6;
        }
        
        .main__recipe-display {
            grid-column: 7/ span 6;
        }
    }
</style>