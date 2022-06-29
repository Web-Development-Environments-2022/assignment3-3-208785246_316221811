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
  <b-form-select @input="getNum()" v-model="selected" :options="options1" />
    </b-row>
<div>
  <multiselect v-model="value" tag-placeholder="Add this as new tag" placeholder="Choose cousines" label="name" track-by="code" :options="options2" :multiple="true" :taggable="true" @tag="addTag"></multiselect>
</div>
<div>
  
<b-form-select @input="sort()"  v-model="search.filter" :options="options"/>
</div>
</div>   
      

    </div> 
<div class="container-fluid">
    </div>
  </div>
</template>

<script>

import RecipePreview from '../components/RecipePreview.vue';
import Multiselect from 'vue-multiselect'

export default {
  name: "Main",
  components: {
        RecipePreview,
        Multiselect },
         props: {
    title: {
      type: String,
      required: true
    }
  },
/* All the data variable declaration are done here:  */
  data() {
    return { 
      selected: "Results number",
      value: [],
      options2: [
        { name: 'African', code: 'African' },
        { name: 'American', code: 'American' },
        { name: 'British', code: 'British' },
        { name: 'Cajun', code: 'Cajun' },
        { name: 'Caribbean', code: 'Caribbean' },
        { name: 'Chinese', code: 'Chinese' },
        { name: 'Eastern European', code: 'Eastern European' },
        { name: 'European', code: 'European' },
        { name: 'Franch', code: 'Franch' },
        { name: 'German', code: 'German' },
        { name: 'Greek', code: 'Greek' },
        { name: 'Indian', code: 'Indian' },
        { name: 'Irish', code: 'Irish' },
        { name: 'Italian', code: 'Italian' },
        { name: 'Japanese', code: 'Japanese' },
        { name: 'Jewish', code: 'Jewish' },
        { name: 'Korean', code: 'Korean' },
        { name: 'Latin American', code: 'Latin American' },
        { name: 'Mediterranean', code: 'Mediterranean' },
        { name: 'Mexican', code: 'Mexican' },
        { name: 'Meadle Eastern', code: 'Meadle Eastern' },
        { name: 'Nordic', code: 'Nordic' },
        { name: 'Southern', code: 'Southern' },
        { name: 'Spanish', code: 'Spanish' },
        { name: 'Thai', code: 'Thai' },
        { name: 'Vietnamese', code: 'Vietnamese' },
      ],
      recipes: [],
      text: '',
       options1: [{ value: "Results number", text: "Results number", disabled:true },
        { value: "5", text: "5" },
        { value: "10", text: "10" },
        { value: "15", text: "15" }
      ],
      options: [
        { value: null, text: "Sort By",disabled: true },
        { value: "preperation time", text: "preperation time" },
        { value: "popularity", text: "popularity" }
      ],
    
     
      search: { filter: null, text: "" },
    };
  },
  methods: {
         addTag (newTag) {
      const tag = {
        name: newTag,
        code: newTag.substring(0, 2) + Math.floor((Math.random() * 10000000))
      }
      this.options.push(tag)
      this.value.push(tag)
    }
  
,
    async search_query() {
      const str1=`${this.myinput}`+""
     
      const num1=this.selected
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          "http://localhost:3000/recipes/searchRecipes",
         { params:{
            query: str1,
            number:num1
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
      console.log(this.search.filter);
      if(this.search.filter == "popularity"){
        this.recipes.sort(function(a, b) {
            return b.aggregateLikes - a.aggregateLikes;
          })}
      else if(this.search.filter == "preperation time"){
        this.recipes.sort(function(a, b) {
            return b.readyInMinutes - a.readyInMinutes;
          })}
      
      

    },
  },
};
</script>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
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