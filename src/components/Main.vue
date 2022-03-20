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
                <p>You can search by the main ingredience and pick for delicious meal - including recipes and ingredience.</p>
                
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

                <h3>Ingredients</h3>

                <div class="instructions__ingredients">
                    <ul> 
                        <li v-if="recipeInstruction.strIngredient1 !== '' ">{{ recipeInstruction.strMeasure1 }}{{ recipeInstruction.strIngredient1 }}</li>
                        <li v-if="recipeInstruction.strIngredient2 !== '' ">{{ recipeInstruction.strMeasure2 }}{{ recipeInstruction.strIngredient2 }}</li>
                        <li v-if="recipeInstruction.strIngredient3 !== '' ">{{ recipeInstruction.strMeasure3 }}{{ recipeInstruction.strIngredient3 }}</li>
                        <li v-if="recipeInstruction.strIngredient4 !== '' ">{{ recipeInstruction.strMeasure4 }}{{ recipeInstruction.strIngredient4 }}</li>
                        <li v-if="recipeInstruction.strIngredient5 !== '' ">{{ recipeInstruction.strMeasure5 }}{{ recipeInstruction.strIngredient5 }}</li>
                        <li v-if="recipeInstruction.strIngredient6 !== '' " >{{ recipeInstruction.strMeasure6 }}{{ recipeInstruction.strIngredient6 }}</li>
                        <li v-if="recipeInstruction.strIngredient7 !== '' " >{{ recipeInstruction.strMeasure7 }}{{ recipeInstruction.strIngredient7 }}</li>
                        <li v-if="recipeInstruction.strIngredient8 !== '' " >{{ recipeInstruction.strMeasure8 }}{{ recipeInstruction.strIngredient8 }}</li>
                        <li v-if="recipeInstruction.strIngredient9 !== '' " >{{ recipeInstruction.strMeasure9 }}{{ recipeInstruction.strIngredient9 }}</li>
                        <li v-if="recipeInstruction.strIngredient10 !== '' " >{{ recipeInstruction.strMeasure10 }}{{ recipeInstruction.strIngredient10 }}</li>
                    </ul>

                    <ul>
                        <li v-if="recipeInstruction.strIngredient11 !== '' " >{{ recipeInstruction.strMeasure11 }}{{ recipeInstruction.strIngredient11 }}</li>
                        <li v-if="recipeInstruction.strIngredient12 !== '' " >{{ recipeInstruction.strMeasure12 }}{{ recipeInstruction.strIngredient12 }}</li>
                        <li v-if="recipeInstruction.strIngredient13 !== '' " >{{ recipeInstruction.strMeasure13 }}{{ recipeInstruction.strIngredient13 }}</li>
                        <li v-if="recipeInstruction.strIngredient14 !== '' " >{{ recipeInstruction.strMeasure14 }}{{ recipeInstruction.strIngredient14 }}</li>
                        <li v-if="recipeInstruction.strIngredient15 !== '' " >{{ recipeInstruction.strMeasure15 }}{{ recipeInstruction.strIngredient15 }}</li>
                        <li v-if="recipeInstruction.strIngredient16 !== '' " >{{ recipeInstruction.strMeasure16 }}{{ recipeInstruction.strIngredient16 }}</li>
                        <li v-if="recipeInstruction.strIngredient17 !== '' " >{{ recipeInstruction.strMeasure17 }}{{ recipeInstruction.strIngredient17 }}</li>
                        <li v-if="recipeInstruction.strIngredient18 !== '' " >{{ recipeInstruction.strMeasure18 }}{{ recipeInstruction.strIngredient18 }}</li>
                        <li v-if="recipeInstruction.strIngredient19 !== '' " >{{ recipeInstruction.strMeasure19 }}{{ recipeInstruction.strIngredient19 }}</li>
                        <li v-if="recipeInstruction.strIngredient20 !== '' " >{{ recipeInstruction.strMeasure20 }}{{ recipeInstruction.strIngredient20 }}</li>
                    </ul>
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
                ingredients: [],
                index: 1,
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
                console.log(this.recipeInstructions);
                this.toggleRecipe();
                
            },

            toggleRecipe() {
                this.isRecipeIntro = !this.isRecipeIntro;
            },
        },
    }
</script>

<style>
    .main {
        width: 100vw;
        height: 100vh;
        overflow: scroll;
        padding-top: 4rem;
        /* padding-bottom: 4rem; */
        display: flex;
        flex-direction: column;   
    }

    .background-image-container {
        height: 100%;
        position: relative;
        background-image: url('/images/fish_curry.jpg');
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center;
    }

    .background-image-container__search {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin: 40px 80px;
        border-radius: 20px; 
        padding: var(--padding-small);
        background-color: var(--background);    
    }

    .background-image-container__search input {
        width: 100%;
        border: none;
        outline: none;
    }

    .recipe-container {
        width: 100%;
        height: 100%;
        padding: var(--padding-medium);
        position: relative;
        text-align: center;
        grid-column: 1/ span 12;
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
        display: block;
        position: fixed;
        top: 200px; 
        bottom: 50px;
        overflow: scroll;
        line-height: 1.7;
        text-align: center;
        overflow-x: scroll;
        background: var(--background);
        padding: var(--padding-big);
        transform: translateY(0);
        transition: all .3s cubic-bezier(.23,1,.32,1);
    }

    .instructions__image {
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
    }

    .instructions__image img {
        width: 100px;
        object-fit: cover;
    }

    .instructions div button img {
        width: 30px;
    }

    .instructions__ingredients {
        display: flex;
        justify-content: space-around;
        align-items: flex-start;
    }

    .instructions__ingredients ul {
        list-style: none;
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
        transform: translateY(-200%);
        transition: all .5s cubic-bezier(.23,1,.32,1); 
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
            margin: 20px;
            overflow: scroll;
            grid-column: 7/ span 6;  
        }

        .instructions {
            top: 70px;
            left: 700px;
            padding: var(--padding-medium)
            /* right: 0px; */
        }
    }
</style>