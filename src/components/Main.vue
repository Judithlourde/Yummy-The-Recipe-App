<template>
    <main class="main">
        <div class="background-image-container">
            <div class="background-image-container__search">
                <input class="" type="text" placeholder="Search" v-model="mainIngredient" @keyup.enter="fetchMenu(); toggleIntro();">

                <button @click="fetchMenu(); toggleIntro();">
                    <img src="/images/svg/search.svg" alt="search-icon">
                </button>
            </div> 
        </div>

        <div class="recipe-container"> 
            <div class="recipe-container__intro" :class="{introVisible: !isIntroVisible}">
                <h3>Dinner Ideas.....</h3>
                <p>Busy week? We can help!You can search and pick for delicious meal - including recipes.</p>
            </div>

            <div class="recipe-container__recipes">
                <Menu 
                    v-for="menu in menus"
                    :menu="menu"
                    :key="menu.id"
                />
            </div>

            <!-- <div class="overlay">
                <p>
                    Lorem ipsum dolor sit amet consectetur adipisicing elit. Dignissimos neque, unde cum et ex veritatis illo repellat incidunt repellendus? Dolore vitae tempora quae, inventore odio est cum eligendi. Facere, minima.
                </p>
            </div> -->
            
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
                mainIngredient: '',
                mealId: '',
                menus: [],
            }
        },

        created() {
            // this.fetchMenu();
        },

        methods: {
            async fetchMenu() {
                const recipeUrl = `https://www.themealdb.com/api/json/v1/1/filter.php?i=${this.mainIngredient}`;
                
                try {
                    const responseMenu = await fetch(recipeUrl);
                    const {meals} = await responseMenu.json();
                    this.menus = meals;
                    console.log(this.menus);
                    console.log(meals);
                    this.mealId = meals[0].idMeal;
                    this.fetchMeal();

                } catch(error) {
                    return 'error';
                }
            },

            // async fetchMeal() {
            //     const ingredienceUrl = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.mealId}`;

            //     try {
            //         const responseDescription = await fetch(ingredienceUrl);
            //         const {meals} = await responseDescription.json();
            //         console.log(meals[0].strInstructions);

            //     } catch(error) {
            //         return 'error';
            //     }
            // },

            toggleIntro() {
                this.isIntroVisible = false;
            }
        }
        
    }
</script>

<style>
    .main {
        padding-top: 4rem;
        padding-bottom: 4rem;
        display: flex;
        flex-direction: column;
        overflow: scroll; 
        width: 100%;
        height: 100%;
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
        margin: 40px;
        background-color: var(--background);
        border-radius: 20px; 
    }

    .background-image-container__search input {
        outline: none;
        border: none;
    }

    .recipe-container {
        grid-column: 1/ span 12;
        height: 100%;
        position: relative;
    }

    .overlay {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;  
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0, 0, 0, 0.6);
        color: white;
        opacity: 0;
        transition: opacity 0.75s; 
        transform: translateY(20px);
        transition: transform 0.75s;
        
        transform: translateY(0px);
        font-size: 34px;
    }

    .overlay:hover > * {
        opacity: 1;
    }

    .recipe-container__recipes {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
        overflow: scroll;
        margin: 20px;
    }

    .recipe-container h3 {
        padding: var(--padding-medium) 0;
    }

    .introVisible {
        position: absolute;
        transform: translateY(-130%);
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

    }
</style>