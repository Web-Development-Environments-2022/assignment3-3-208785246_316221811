<template>
  <div class="container">
    <h1 class="title">Family recipes</h1>
  </div>
</template>

<script>
export default {
    name: "FamilyRecipes",
    data() {
    return {
      familyRecipes: []
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
        console.log(response);
        const recipes = response.data;
        console.log(recipes);
        this.familyRecipes = [];
        this.familyRecipes.push(...recipes);
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
.container {
  max-width: 500px;
}
</style>