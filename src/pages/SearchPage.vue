<template>
  <div>
    <div class="pad-15-hor pad-15-ver search-parent">
      <div class="search-bar">
<b-form-input
          v-model="myinput"
          type="text"
          placeholder="Search by Name"
        ></b-form-input></div>
      <div><b-button id="search" @click="search_query">search</b-button></div>
      <div>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
        <b-form-select @input="sort()" v-model="search.filter" :options="options"/>
      </div>
    </div>
<div class="container-fluid">

    </div>
  </div>
</template>

<script>
import RecipePreview from '../components/RecipePreview.vue';
export default {
  name: "Main",
  components: {
        RecipePreview },
/* All the data variable declaration are done here:  */
  data() {
    return {
      recipes: [],
      text: '',
      options: [
        { value: null, text: "Sort By" },
        { value: "a", text: "preperation time" },
        { value: "b", text: "popularity" }
      ],
      search: { filter: null, text: "" },
      likes: { count: 0, hit: 0 }
    };
  },
  methods: {
    async search_query() {
      const str1=(`${this.myinput}`)
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          "http://localhost:3000/recipes/searchRecipes",
         { params:{
            query: str1
          }}
        );
        this.axios.defaults.withCredentials = false;
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);

      } catch (err) {
        console.log(err.response);
      }
    },
    sort() {
      //console.log(this.search.filter);
      this.search.filter == "b"
        ? this.wonders_data.sort(function(a, b) {
            return b.likes - a.likes;
          })
        : this.wonders_data.sort(function(a, b) {
            return b.ratings - a.ratings;
          });
    },
  },
};
</script>
<style scoped> 
.search-parent {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
  background-color: lightgray;
}

.search-bar {
  position: relative;
}
.search-bar input {
  padding-left: 30px;
  width: 400px;
}
.search-icon {
  position: absolute;
  top: 8px;
  left: 8px;
}
</style>