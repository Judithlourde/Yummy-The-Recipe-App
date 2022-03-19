<template>
    <main class="main">
        <div class="background-image-container">
            <div class="background-image-container__search">
                <button @click="fetchMenu(); toggleIntro();">
                    <img src="/images/svg/search.svg" alt="search-icon">
                </button>

                <input class="" type="text" placeholder="Search by main ingredience" v-model="mainIngredient" @keyup.enter="fetchMenu(); toggleIntro();">   
            </div> 
        </div>
        
        <div class="recipe-container"> 
            <div class="recipe-container__intro" :class="{introVisible: !isIntroVisible}">
                <h3>Dinner Ideas.....</h3>
                <p>Busy week? </p>
                <p>We can help!</p>
                <p>You can search and pick for delicious meal - including recipes and ingredience.</p>
                
            </div>

            <p>{{ meassage }}</p>
            <p>{{ error }}</p>

            <div class="recipe-container__recipes">
                <Menu
                    v-for="menu in menus"
                    :menu="menu"
                    :key="menu.id"
                    @get-recipe-instruction="getInstruction"
                    @get-recipe="toggleRecipe"
                />
            </div>

            <div class="recipe-container__instructions" v-for="recipeInstruction in recipeInstructions" :key="recipeInstruction.id" :class="{instructions: isRecipeIntro}">
                <div class="instructions__image">
                    <img :src="recipeInstruction.strMealThumb" alt="recipe-image">

                    <h4>{{ recipeInstruction.strMeal }}</h4>

                    <button @click="toggleRecipe">
                        <img src="/images/svg/close.svg" alt="close-icon">
                    </button>
                </div>
                
                <div>
                    <h4>Ingredience</h4>
                    <p>{{ recipeInstruction.strIngredient5 }}</p>
                </div>

                <p>{{ recipeInstruction.strInstructions }}</p>
        
                <a :href="recipeInstruction.strYoutube">Youtube Link</a>
            </div>
            
        </div>
    </main>
</template>

<script>
import Menu from '../components/Menu.vue'
    export default {
        components: {
            Menu,
        },

        data() {
            return {
                isIntroVisible: true,
                isRecipeIntro: false,
                mainIngredient: '',
                mealId: '',
                menus: [],
                recipeInstructions: [],
                error: '',
                message: '',
            }
        },

        created() {
            // this.fetchMenu();
        },

        methods: {
            async fetchMenu() {
                
                const recipeUrl = `https://www.themealdb.com/api/json/v1/1/filter.php?i=${this.mainIngredient}`;
                const responseMenu = await fetch(recipeUrl);

                try {
                    await this.handleResponse(responseMenu);
            
                } catch(error) {
                    this.error = error.message;
                }
            },

            async handleResponse(responseMenu) {
                if(responseMenu.status >= 200 && responseMenu.status < 300) {
                    console.log(responseMenu);
                    const {meals} = await responseMenu.json();
                    this.menus = meals;
                    if(meals === null) {
                        this.meassage = 'Unauthorized';
                    }
                    console.log(meals);
                    this.mealId = meals[0].idMeal;
                    // this.fetchMeal();
                    
                } else {
                    if(responseMenu.status === 404) {
                        console.log('Sorry, Page could not be found. Please write the correct place.');
                        throw new Error('Sorry, Weather could not be found. Please write the correct place.');
                    }
                    if(responseMenu.status === 401) {
                        console.log('Unauthorized');
                        throw new Error('Unauthorized');
                    }
                    if(responseMenu.status > 500) {
                        console.log('Servor Error!');
                        throw new Error('Servor Error!');
                    }
                    console.log('Something went wrong!');
                    throw new Error('Something went wrong!');
                }
            },

            toggleIntro() {
                this.isIntroVisible = false;
            },

            getInstruction(recipe) {
                this.recipeInstructions = recipe;
                this.toggleRecipe();
            },

            toggleRecipe() {
                this.isRecipeIntro = !this.isRecipeIntro;
            },
        }   
    }
</script>

<style>
    .main {
        padding-top: 4rem;
        /* padding-bottom: 4rem; */
        display: flex;
        flex-direction: column;
        overflow: scroll; 
        width: 100vw;
        height: 100vh;
    }

    .background-image-container {
        height: 100%;
        background-image: url('/images/fish_curry.jpg');
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center;
        position: relative;
        /* overflow: hidden; */
    }

    .background-image-container__search {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: var(--padding-small);
        margin: 40px 80px;
        background-color: var(--background);
        border-radius: 20px; 
    }

    .background-image-container__search input {
        outline: none;
        border: none;
        width: 100%;
    }

    .recipe-container {
        grid-column: 1/ span 12;
        height: 100%;
        width: 100%;
        position: relative;
    }

    .recipe-container__instructions {
        display: none;
        position: absolute;
        /* transform: translateY(-150%); */
        transition: all .3s cubic-bezier(.23,1,.32,1); 
    }

    .recipe-container__recipes {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 10px;
        /* overflow: scroll; */
        margin: 20px;
    }

    .instructions {
        overflow: scroll;
        transform: translateY(0);
        transition: all .3s cubic-bezier(.23,1,.32,1);
        display: block;
        position: fixed;
        top: 200px; 
        bottom: 50px;
        background: var(--background);
        padding: 40px;
        text-align: center;
        overflow-x: scroll;
        line-height: 1.5;
    }

    .instructions__image {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
    }

    .instructions__image img {
        width: 100px;
        object-fit: cover;
    }

    .instructions div button img {
        width: 30px;
    }

    .recipe-container__intro {
        /* width: 500px; */
        line-height: 2;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .recipe-container h3 {
        padding: var(--padding-medium) 0;
    }

    .introVisible {
        position: absolute;
        transform: translateY(-160%);
        transition: all .3s cubic-bezier(.23,1,.32,1); 
    }

    /* Medium screen devices (968px and above) */
    @media screen and (min-width: 968px) {
        .main {
            display: grid;
            grid-template-columns: var(--grid-column-12);
        }

        .background-image-container {
            grid-column: 1/ span 6;
        }
        
        .recipe-container {
            grid-column: 7/ span 6;
            overflow: scroll;
            margin: 20px;
        }

        .instructions {
            top: 50px;
            left: 700px;
            /* right: 0px; */
        }
    }
</style>