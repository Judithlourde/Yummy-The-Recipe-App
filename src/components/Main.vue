<template>
    <main role="main" class="main">
        <div class="background-image-container">
            <div class="background-image-container__search">
                <button aria-label="search" @click="fetchMenu(); toggleIntro();">
                    <img src="/images/svg/search.svg" alt="search-icon">
                </button>

                <input type="text" placeholder="Search by main ingredient or just enter for random recipes" v-model="mainIngredient" @keyup.enter="fetchMenu(); toggleIntro();">   
            </div> 
        </div>
        
        <div class="recipe-container"> 
            <div class="recipe-container__intro" :class="{introVisible: !isIntroVisible}">
                <h1>Dinner Ideas.....</h1>
                <h3>Busy week? </h3>
                <h3>Yummy <span> by Mummy </span> can help!</h3>
                <p>You can search by the main ingredient for examples Salmon, Chicken, Flour, Tomato, egg, milk etc... and pick for delicious meal - including recipes with ingredience.</p>
                <span>Cooking is an art, food gratifies the audience when we cook with a secret ingredient called “Love”.</span>    
            </div>

            <p class="recipe-container__suggestion">{{ message }}</p>
            <p v-if="isErrorVisible">{{ error }}</p>
            <p>{{ recipeError }}</p>

            <div class="recipe-container__recipes">
                <!-- Looping the menus array and sending the object by props to the child component (Menu) -->
                <!-- And getting the recipe instruction from Menu component by emit -->
                <Menu
                    v-for="menu in menus"           
                    :menu="menu"
                    :key="menu.id"
                    
                    @get-recipe-instruction="getInstruction" 
                    @toggle-recipe="toggleRecipe"
                    @get-recipe-error='recipeErrorMessage'
                />
            </div>

            <div class="recipe-container__instructions" v-for="recipeInstruction in recipeInstructions" :key="recipeInstruction.id" :class="{instructions: isRecipeIntro}">
                <div class="instructions__image">
                    <img :src="recipeInstruction.strMealThumb" alt="recipe-image">

                    <h4>{{ recipeInstruction.strMeal }}</h4>

                    <button aria-label="close"  @click="toggleRecipe">
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
                    </ul>

                    <ul>    
                        <li v-if="recipeInstruction.strIngredient9 !== '' " >{{ recipeInstruction.strMeasure9 }}{{ recipeInstruction.strIngredient9 }}</li>
                        <li v-if="recipeInstruction.strIngredient10 !== '' " >{{ recipeInstruction.strMeasure10 }}{{ recipeInstruction.strIngredient10 }}</li>
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
                <a aria-label="youtube link" :href="recipeInstruction.strYoutube">Youtube Link</a>
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
                isErrorVisible: false,
                mainIngredient: '',
                mealId: '',
                menus: [],
                recipeInstructions: [],
                error: '',
                message: '',
                ingredients: [],
                recipeError: '',
            }
        },

        methods: {
            // This function fetching the menu data with main ingredient som given by user in search bar
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
                    this.message = ''
                    const { meals } = await responseMenu.json();
                    this.menus = meals;
                    console.log(meals);
                    // If the main ingredients not found when user search still response is 200 and meals === null. Then it shows the suggestion message. 
                    if(this.menus === null) {   
                        this.message = `We don't have ${this.mainIngredient} in our meal. But you can find many other recepies by main ingredient. Such as Salmon, Chicken, Egg, Onion, Tomato, Flour...`;
                    }

                    // Setting the meal id to variable, it helps to fetch the ingredients and instructions from the other URL
                    this.mealId = meals[0].idMeal;  
                    // this.fetchMeal();
                    
                } else {
                    this.errorVisible();    // Calling the toggle error function when error occured
                    if(responseMenu.status === 404) {
                        throw new Error('Sorry, Page could not be found');
                    }
                    if(responseMenu.status === 401) {
                        throw new Error('Unauthorized');
                    }
                    if(responseMenu.status > 500) {
                        throw new Error('Servor Error!');
                    }
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

            //  Need this toggle error function to visible the error message only when error occured
            errorVisible() {
                this.isErrorVisible = !this.isErrorVisible;
            },
            recipeErrorMessage(recipeError) {
                this.recipeError = recipeError;
            }
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
        width: 100%;
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
        margin: 40px 70px;
        border-radius: 25px; 
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
        transition: all .3s cubic-bezier(.23,1,.32,1); 

    }

    .recipe-container__instructions a {
        color: inherit;
    }

    .recipe-container__recipes {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: var(--gap-xsmall);
    }

    .recipe-container__suggestion {
        padding-top: var(--top-small);
        line-height: 2;
    }

    .instructions {
        display: block;
        position: fixed;
        top: 60px; 
        bottom: 20px;
        right: 0;
        overflow: scroll;
        line-height: 1.7;
        text-align: center;
        overflow-x: scroll;
        background: var(--background);
        padding: var(--padding-medium);
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
        line-height: 1.5;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding-bottom: var(--padding-small);   
    }

    .recipe-container__intro span {
        font-style: italic;
        font-weight: 800;
    }

    .recipe-container h3 {
        padding: var(--padding-small) 0;
    }

    .introVisible {
        position: absolute;
        transform: translateY(-240%);
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

        .recipe-container__suggestion {
            position: absolute;
            top: 230px;
            left: 50px;
            right: 50px;
        }
        
        .recipe-container {
            margin: var(--margin-small);
            overflow: scroll;
            grid-column: 7/ span 6;  
        }

        .instructions {
            top: 70px;
            bottom: 0;
            left: 650px;
            padding: var(--padding-medium);
        }

        .recipe-container__intro {
            width: 580px;
            line-height: 2;
            height: 80vh;
        }
    }
</style>