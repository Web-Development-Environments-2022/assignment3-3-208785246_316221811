<template>
  
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div><h6>Ready in:</h6> {{ recipe.readyInMinutes }} minutes</div>
              <div><h6>Likes:</h6> {{ recipe.popularity }} likes</div>
              <div><h6>servings:</h6>{{ recipe.servings }} </div>
            </div>
          <img v-if="recipe.vegan" class="vegan" src="../assets/vega.jpg" />
          <img v-if="recipe.vegetarian" class="vegan" src="../assets/ve.png" />
          <img v-if="recipe.glutenFree" class="vegan" src="../assets/gl.jpg" />
            <h6>Ingredients:</h6>
            <ul>
              <li
                v-for="(r, index) in recipe.extendedIngredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
          </div>
          <div class="wrapped">
            <h6>Instructions:</h6>
            <ol>
              <li v-for="s in recipe._instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
          </div>
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null
    };
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;

      try {
        response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/recipes/info",
          // this.$root.store.server_domain + "/recipes/info",
          "http://localhost:3000/recipes/recipeId",
          {
            params: { recipeId: this.$route.params.recipeId }
          }
        );

        // console.log("response.status", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

      let {
        analyzedInstructions,
        instructions,
        extendedIngredients,
        popularity,
        readyInMinutes,
        image,
        title,
        servings,
        vegan,
        vegetarian,
        glutenFree
      } = response.data;

     let _instructions = analyzedInstructions
        .map((fstep) => {
          fstep.steps[0].step = fstep.name + fstep.steps[0].step;
          return fstep.steps;
        })
        .reduce((a, b) => [...a, ...b], []);

      let _recipe = {
        instructions,
        _instructions,
        analyzedInstructions,
        extendedIngredients,
        popularity,
        readyInMinutes,
        image,
        title,
        servings,
        vegan,
        vegetarian,
        glutenFree
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}

.vegan {
  width: 50px;
}

h1 { color: rgb(120, 166, 203); text-shadow: 2px 2px 2px #000000; font-family: 'Raleway',sans-serif; font-size: 40px; font-weight: 800; text-align: center; text-transform: uppercase; }
h6 { color: rgb(120, 166, 203); font-family: 'Raleway',sans-serif; font-size: 20px; font-weight: 800; }

/* .recipe-header{

} */
</style>
