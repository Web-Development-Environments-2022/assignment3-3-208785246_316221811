<template>
  <div>
    <div><b-button v-bind:id="recipe.id" v-if="$root.store.username && recipe.id<12345678" @click="markAsFavorite">favorite</b-button>
    </div>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id } }" @click.native="RecipeIsViewed" id="clickPhoto"
    class="recipe-preview"
  >
    <div class="recipe-body">
      <img v-if="image_load" :src="recipe.image" class="recipe-image" />
    </div>
    <div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title">
        {{ recipe.title }}
      </div>
      <ul class="recipe-overview">
        <li>{{ recipe.readyInMinutes }} minutes</li>
        <br>
        <li>{{ recipe.aggregateLikes || recipe.popularity }} likes</li>
        <li>
          <img v-if="recipe.vegan" class="vegan" src="../assets/vega.jpg" />
        </li>
        <li>
          <img v-if="recipe.vegetarian" id="vegetarian" class="vegan" src="../assets/ve.png" />
        </li>
        <li>
          <img v-if="recipe.glutenFree" class="vegan" src="../assets/gl.jpg" />
        </li>
      </ul>
    </div>
  </router-link>
    <br />
    <br />
    <br />
    <br />
    <br />
  </div>
</template>

<script>

export default {
  mounted() {
    this.axios.get(this.recipe.image).then((i) => {
      this.image_load = true;
    });
    if (localStorage.getItem("username") && this.favorites1.includes(this.recipe.id)){
        document.getElementById(this.recipe.id).style.backgroundColor = "green";
        document.getElementById(this.recipe.id).disabled = true;
      }
  },
  
  data() {
    return {
      favorites1: localStorage.getItem("favorites"),
      isFavorite: false,
      image_load: false,
      viewed: [],
    };
  },
  methods: {
    async RecipeIsViewed(){
      try {
        this.axios.defaults.withCredentials = true;
        response = await this.axios.post(
          "http://localhost:3000/users/viewed",
          {
            recipeId: this.recipe.id 
          }
        );
        this.axios.defaults.withCredentials = false;
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (err) {
        console.log(err.response);
      }
    },
    async markAsFavorite() {
      try {
        console.log(this.favorites1);
        document.getElementById(this.recipe.id).style.backgroundColor = "green";
        document.getElementById(this.recipe.id).disabled = true;
        this.isFavorite = true;
        localStorage.setItem("favorites", localStorage.getItem("favorites").concat("_").concat(this.recipe.id));
        this.axios.defaults.withCredentials = true;
        response = await this.axios.post(
          "http://localhost:3000/users/favorites",
          {
            recipeId: this.recipe.id 
          }
        );
        this.axios.defaults.withCredentials = false;
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (err) {
        console.log(err.response);
      }
    },
  },
  props: {
    recipe: {
      type: Object,
      required: true,
      id: {
        type: Number,
        required: true,
      },
      title: {
        type: String,
        required: true,
      },
      readyInMinutes: {
        type: Number,
        required: true,
      },
      image: {
        type: String,
        required: true,
      },
      popularity: {
        type: Number,
        required: false,
        default() {
          return undefined;
        },
      },
    },
  },
};
</script>

<style scoped>

.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 90%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 90%;
  top:-20px;
  height: 200px;
  position: relative;
}

#view {
  border-radius: 50%;
}

#vegetarian {
  border-radius: 50%;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: 0px;
  margin-right: auto;
  margin-top: 20px;
  margin-bottom: auto;
  display: block;
  width: 77%;
  max-height: 200px;
  max-width: 320px;
  border-radius: 20px;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
  
}

.recipe-preview .recipe-footer {
  width: 50%;
  height: 50%;
  overflow: hidden;
  font-family: 'Raleway',sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.vegan {
  width: 50px;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 70%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}

#clickPhoto { 
    opacity: 1;
    cursor: pointer;
} 
#clickPhoto:hover{ 
    opacity: 0.5;
}

</style>