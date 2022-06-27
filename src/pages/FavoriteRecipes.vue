<template>
  <div class="container">
    <h1 class="title">favorite recipes</h1>
    <b-row>
      <b-col v-for="r in recipes" :key="r.recipe_id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </div>
</template>

<script>
import RecipePreview from '../components/RecipePreview.vue';
export default {
    name: "FavoriteRecipes",
    components: {
        RecipePreview },
    props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
        //  this.$root.store.server_domain + "/recipes/random",
          "http://localhost:3000/users/favorites",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        this.axios.defaults.withCredentials = false;
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
         console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>