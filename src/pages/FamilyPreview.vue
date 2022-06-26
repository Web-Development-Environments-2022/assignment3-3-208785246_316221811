<template>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.recipe_id } }"
    class="recipe-preview"
  >
    <div class="recipe-body">
      <img v-if="image_load" :src="recipe.imageurl" class="recipe-image" />
    </div>
    <div class="recipe-footer">
      <div :gradients="recipe.gradients" class="recipe-title">
        {{ recipe.gradients }}
      </div>
    </div>
  </router-link>
</template>

<script>
export default {
  mounted() {
    this.axios.get(this.recipe.imageurl).then((i) => {
      this.image_load = true;
    });
  },
  data() {
    return {
      image_load: false,
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true,
      owner: {
        type: String,
        required: true,
      },
      time: {
        type: String,
        required: true,
      },
      gradients: {
        type: String,
        required: true,
      },
      prepararion: {
        type: String,
        required: true,
      },
      imageurl: {
        type: String,
        required: false,
      },
    },
  },
};
</script>

<style>

</style>
