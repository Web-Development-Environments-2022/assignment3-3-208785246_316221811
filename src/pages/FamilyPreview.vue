<template>
  <div class="container">
    <div v-if="recipe">
      <h6>owner:</h6>
      <div :owner="recipe.owner" class="recipe-owner">
        {{ recipe.owner }}
      </div>
      <h6>time:</h6>
      <div :time="recipe.time" class="recipe-time">
        {{ recipe.time }}
      </div>
      <h6>ingredients:</h6>
      <div :gradients="recipe.gradients" class="recipe-ingredients">
        {{ recipe.gradients }}
      </div>
      <h6>preparation:</h6>
      <div :prepararion="recipe.preparation" class="recipe-prepararion">
        {{ recipe.preparation }}
    </div>
      <div class="recipe-body">
      <img v-if="image_load" :src="recipe.imageurl" class="recipe-image" />
      </div>
      </div>
      </div>
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
h6{ color: #5d58e1; font-family: "Great Vibes", cursive; font-size: 20px; font-weight: normal; text-shadow: 0 1px 1px #fff; }
</style>
