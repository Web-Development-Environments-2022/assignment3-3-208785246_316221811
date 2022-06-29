<template>
  <div class="container">
    <h1 class="title">Family recipes</h1>
    <b-row>
      <b-col v-for="r in recipes" :key="r.recipe_id">
        <FamilyPreview class="FamilyPreview" :recipe="r" />
      </b-col>
    </b-row>
  </div>
</template>

<script>
import FamilyPreview from "./FamilyPreview.vue"
export default {
      name: "FamilyRecipes",
      components: {
      FamilyPreview
    },
    data() {
    return {
      recipes: []
    };
  },
  methods: {
    async getRecipes() {
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          "http://localhost:3000/users/family"
        );
        this.axios.defaults.withCredentials = false;
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
      } catch (err) {
        console.log(err.response);
      }
    }
  },
  mounted() {
    this.getRecipes();
  }
};


</script>

<style lang="scss" scoped>
h1 { color: #ffffff; text-shadow: 2px 2px 2px #000000; font-family: 'Raleway',sans-serif; font-size: 40px; font-weight: 800; text-transform: uppercase; text-align: center; }
.container {
  max-width: 1500px;
}
</style>