<template>
  <div class="container">
    <div v-if="this.recipes[0]">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ this.recipes[0].title }}</h1>
        <img :src="this.recipes[0].image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div><h6>Ready in:</h6> {{ this.recipes[0].readyInMinutes }} minutes</div>
              <div><h6>Likes:</h6> {{ this.recipes[0].popularity }} likes</div>
              <div><h6>servings:</h6>{{ this.recipes[0].servings }} </div>
            </div>
          <img v-if="this.recipes[0].vegan" class="vegan" src="../assets/vega.jpg" />
          <img v-if="this.recipes[0].vegetarian" class="vegan" src="../assets/ve.png" />
          <img v-if="this.recipes[0].glutenFree" class="vegan" src="../assets/gl.jpg" />
            <h6>Ingredients:</h6>
            {{ this.recipes[0].ingredients }}
          </div>
          <div class="wrapped">
            <h6>Instructions:</h6>
             {{ this.recipes[0].instructions }}
          </div>
        </div>
      </div>
      </div>
      </div>
</template>

<script>
export default {
     data() {
    return {
      recipes: [],
    };
  },
    mounted() {
    this.getMyRecipe();
  },
    methods: {
    async getMyRecipe(){
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          "http://localhost:3000/users/myrecipe",
          {
            params: { recipeId: this.$route.params.recipeId }
          }
        );
        this.axios.defaults.withCredentials = false;
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
      } catch (err) {
        console.log(err.response);
      }
    },
    },
};
</script>

<style>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}

.vegan {
  width: 50px;
}

h1 { color: rgb(120, 166, 203); text-shadow: 2px 2px 2px #000000; font-family: 'Raleway',sans-serif; font-size: 40px; font-weight: 800; text-align: center; text-transform: uppercase; }
h6 { color: rgb(120, 166, 203); font-family: 'Raleway',sans-serif; font-size: 20px; font-weight: 800; }

/* .recipe-header{

} */
h6 { color: rgb(120, 166, 203); font-family: 'Raleway',sans-serif; font-size: 20px; font-weight: 800; }

#recipe_photo{ width:200px; border-radius: 20%;}
</style>
