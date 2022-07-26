<template>
  <div>
     
    <div class="pad-15-hor pad-15-ver search-parent">
      <div id="search-bar" class="search-bar">
<b-form-input
          v-model="myinput"
          type="text"
          placeholder="Search by Name"
        ></b-form-input></div>
      <div><b-button class="button" id="search"  @click="search_query">search</b-button></div>
      <div>
  
  <b-form-select id="numchoose" class="select" @input="getNum()" v-model="selected" :options="options1" />
<div>
  <multiselect v-model="value" id="cuisinechoose" class="select" tag-placeholder="Add this as new tag" placeholder="Choose cousines" label="name" track-by="code" :options="options2" :multiple="true" :taggable="true" @tag="addTag"></multiselect>
</div>

</div>   
      

    </div> 
<div class="container-fluid">
    <b-col >
        <b-form-select @input="sort()" id="sortchoose" v-model="search.filter" :options="options"/>

      <b-row class="recipePreview" v-for="r in recipes" :key="r.id">
        <RecipePreview :recipe="r" />

      </b-row>
  
        </b-col>

    </div>
  </div>
</template>

<script>

import RecipePreview from '../components/RecipePreview.vue';
import Multiselect from 'vue-multiselect'
//import { VueModalDialog } from 'vue-modal-dialog';


 
//Vue.use('VueModalDialog');

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

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background: rgb(202, 222, 229);
}

.search-bar {
  padding-left: 30px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

}

.select{
  position: relative;
  width: 170px;
  right:200px;
  top:-10px;
  font-family: 'Raleway',sans-serif;
  text-align: center;

}
#numchoose{
  position: relative;
  right: 390px;
  height: 43px;
  top:33.4px; 
  color: rgb(170, 161, 161);
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  font-size: smaller;
}

#sortchoose{
  position: relative;
  padding-left: 30px;
  width: 190px;
  left:1000px;
  top:250px;
  font-family: 'Raleway',sans-serif;
}

.select input{
  padding-left: 40px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  

  background:(120, 166, 203);

}

#search-bar{
  position:absolute;
  top: 694px;
  width: 500px;
}

#search{
position: absolute;
top: 694px;
left: 515px;
background-color: #3f5874;

}
.button:hover{
  transform: scale(1.2,1.2);
}
.container-fluid{
  position:relative;
  min-height: 450px;
  align-items: center;
}

#recipePreview{
  position:absolute;
  left:800px;
  align-items: center;
  align-self: center;
}

.search-bar input {
  padding-left: 30px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  background:(120, 166, 203);
}

</style>