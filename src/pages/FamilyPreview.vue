<template>
  <div class="container">
    <div v-if="recipe">
      <img id="recipe_photo" :src="require(`../assets/${recipe.recipe_id}.jpeg`)">
      <h6>owner:</h6>
      <div :owner="recipe.owner" class="recipe">
        {{ recipe.owner }}
      </div>
      <h6>time:</h6>
      <div :time="recipe.time" class="recipe">
        {{ recipe.time }}
      </div>
      <h6>ingredients:</h6>
      <div :gradients="recipe.gradients" class="recipe">
        {{ recipe.gradients }}
      </div>
      <h6>preparation:</h6>
      <div :prepararion="recipe.preparation" class="recipe">
        {{ recipe.preparation }}
    </div>
      </div>
      </div>
</template>

<script>
export default {
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
      preparation: {
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
h6 { color: rgb(120, 166, 203); font-family: 'Raleway',sans-serif; font-size: 20px; font-weight: 800; }

#recipe_photo{ width:200px; border-radius: 20%;}
</style>
