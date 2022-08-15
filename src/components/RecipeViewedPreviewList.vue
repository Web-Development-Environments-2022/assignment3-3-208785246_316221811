<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <img class="vegan" id="view" src="../assets/view.jpg" />
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipeViewedPreviewList",
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
      recipes: []
    };
  },
  mounted() {
    this.updateViewedRecipes();
  },
  methods: {
    async updateViewedRecipes() {
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
        //  this.$root.store.server_domain + "/recipes/random",
          "http://localhost:3000/users/ThreeLastRecipes",
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
  min-height: 400px;
}

#view {
  margin:0px;
  margin-top: -30px;
  padding:0px;
  position:relative;
  left:90px;
  top:33px;
  border-radius: 50%;
  width: 30px;
}

</style>
