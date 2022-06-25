<template>
  <div class="container">
    <h1 class="title">favorite recipes</h1>
    <RecipePreview>
    </RecipePreview>
  </div>
</template>

<script>
import RecipePreview from '../components/RecipePreview.vue';
export default {
    name: "FavoriteRecipes",
    components: {
        RecipePreview }
}

</script>

<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>