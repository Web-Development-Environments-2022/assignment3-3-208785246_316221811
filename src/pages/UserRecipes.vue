<template>
  <b-container>
    <h1> My recipes</h1>
    <b-row>
      <b-col v-for="r in recipes" :key="r.recipe_id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";
export default {
  name: "myrecipes",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
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
          "http://localhost:3000/users/myrecipe",
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
h1 { color: #ffffff; text-shadow: 2px 2px 2px #000000; font-family: 'Raleway',sans-serif; font-size: 40px; font-weight: 800; text-align: center; text-transform: uppercase; }
</style>
