<template>
<div>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r"/>
      </b-row>
    </b-col>
  </b-container>
  <b-button id="showme" @click="showMore">Show me more!</b-button>
</div>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: [],
      favorite_recipes: []
    };
  },
    mounted() {
    //this.favoriteRecipes();
    this.updateRecipes();
  },
  methods: {
    async showMore() {
      this.updateRecipes();
    },
    async favoriteRecipes() {
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          "http://localhost:3000/users/favorites",
        );
        this.axios.defaults.withCredentials = false;
        console.log(response);
        const favorite_recipes = response.data;
        this.favorite_recipes = [];
        //this.favorite_recipes.push(...favorite_recipes);
        for (let i=0; i<favorite_recipes.length; i++){
          this.favorite_recipes.push(favorite_recipes[i].id);
        }
        console.log(this.favorite_recipes);
      } catch (error) {
        console.log(error);
      } 
    },
    async updateRecipes() {
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
        //  this.$root.store.server_domain + "/recipes/random",
          "http://localhost:3000/recipes/GetRandomRecipes?num=3",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        this.axios.defaults.withCredentials = false;
        //console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}

#showme{
  background-color: rgb(83, 114, 139);
  margin:10px;
  position: relative;
  bottom: 10px;
}
</style>
