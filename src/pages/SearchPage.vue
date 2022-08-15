<template>
  <div>
     
    <div class="pad-15-hor pad-15-ver search-parent">
      <div id="search-bar" class="search-bar"  @click="onClick">
<b-form-input
          v-model="myinput"
          type="text"
          placeholder="Search by Name"
        ></b-form-input></div>
        <b-button @click="chooserecent" v-if="showSearchHistory" id="recentsearch" > {{lastSearch}}</b-button>
      <div><b-button class="button" id="search"  @click="search_query">search</b-button></div>
      <div> 
  
  <b-form-select id="numchoose" class="select" v-model="selected" :options="options1" />
<div>
  <multiselect v-model=value id="cuisinechoose" tag-placeholder="Add this as new tag" placeholder="Choose cousine" label="name" track-by="code" :options="options2" :multiple="false" :taggable="true" @tag="addTag"></multiselect>
</div>
<div>
  <b-form-select id="Intolerances" v-model="chosenintolerances" :options="options3" />
</div>
</div>   
      

    </div> 
<div class="container-fluid">
    <b-col >
      <div v-if="recipes.length&&sent"> <h3>Here are {{selected}} recipes for {{myinput}} </h3><h3 v-if="this.chosenintolerances"> without {{this.chosenintolerances}}:</h3>
        <h5 id="h5">sort results by:</h5><b-form-select @input="sort()" id="sortchoose" v-model="search.filter" :options="options"/>
      <b-row class="recipePreview" v-for="r in recipes" :key="r.id">
        <RecipePreview :recipe="r" />

      </b-row>
      </div>
  <div v-else-if="!recipes.length&&sent"><h3> There are no recipes matching your request! Please try again.</h3></div>

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

/* All the data variable declaration are done here:  */
  data() {
    return {
      Searclasth: localStorage.getItem("last_search"), 
      showSearchHistory: false,
      sent: false,
      value:[],
      myinput: "",
      chosenintolerances:null,
      selected: "5",
      value2: [],
      options2: [
        { name: 'African', code: 'African' },
        { name: 'American', code: 'American' },
        { name: 'British', code: 'British' },
        { name: 'Cajun', code: 'Cajun' },
        { name: 'Caribbean', code: 'Caribbean' },
        { name: 'Chinese', code: 'Chinese' },
        { name: 'Eastern European', code: 'Eastern European' },
        { name: 'asxc European', code: 'European' },
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

      value1: [],
      options3: [ { value: null, text: "Choose Intolerances",disabled: false },
        { value: 'Dairy', text: 'Dairy' },
        { value: 'Egg', text: 'Egg' },
        { value: 'Gluten', text: 'Gluten' },
        { value: 'Grain', text: 'Grain' },
        { value: 'Peanut', text: 'Peanut' },
        { value: 'Seafood', text: 'Seafood' },
        { value: 'Sesame', text: 'Sesame' },
        { value: 'Shellfish', text: 'Shellfish' },
        { value: 'Soy', text: 'Soy' },
        { value: 'Sulfite', text: 'Sulfite' },
        { value: 'Tree Nut', text: 'Tree Nut' },
        { value: 'Wheat', text: 'Wheat' },
        
      ],
      recipes: [],
      text: '',
       options1: [{ value: null, text: "Results Number",disabled: true },
        { value: "5", text: "5" },
        { value: "10", text: "10" },
        { value: "15", text: "15" }
      ],
      options: [
        { value: null, text: "Sort By",disabled: true },
        { value: "preparation time", text: "preparation time" },
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
    },
    onClick() {
        // Toggle show/hide
        this.showSearchHistory = !this.showSearchHistory;
      },
    addTag1 (newTag1) {
      const tag = {
        name: newTag1,
        code: newTag1.substring(0, 2) + Math.floor((Math.random() * 10000000))
      }
      
      this.options3.push(tag)
      this.value1.push(tag)
    },
    chooserecent(){
        this.myinput=localStorage.getItem("last_search");
        this.showSearchHistory = false;
      },

    async search_query() {
      localStorage.setItem("last_search", this.myinput);
      this.lastSearch = this.myinput;
      this.showSearchHistory = false
      this.search.filter = "Sort By"
      const str1=`${this.myinput}`+""
      const num1=this.selected
      const intol=`${this.chosenintolerances}`
      const cuis= this.value
      try {
        this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          "http://localhost:3000/recipes/searchRecipes",
         { params:{
            query: str1,
            number:num1,
            intolerances: intol,
            cuisine: cuis
          }}
        );
        this.axios.defaults.withCredentials = false;
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        this.sent=true

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
      if(this.search.filter == "preparation time"){
        this.recipes.sort(function(a, b) {
            return a.readyInMinutes- b.readyInMinutes;
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
  padding-top: 11px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

}

#cuisinechoose{
  position: relative;
  right: 205px;
  top:  -53px;
  height: 43px;
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  font-size: smaller;

}

#Intolerances{
  position: relative;
  right: 194px;
  top:  -43px;
  height: 43.5px;
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  font-size: smaller;


}
#numchoose{
  position: relative;
  right: 390px;
  height: 43.5px;
  top:43px; 
  color: Black(170, 161, 161);
  text-align: center;
  font-family: 'Raleway',sans-serif;
  font-size: smaller;
}
.select{
  position: relative;
  width: 170px;
  top:-10px;
  font-family: 'Raleway',sans-serif;
  text-align: center;

}
#sortchoose{
  position: relative;
  padding-left: 30px;
  width: 190px;
  left:500px;
  top:-50px;
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
top: 705px;
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
    align-items: center;
  align-self: center;

}

#recipePreview{
  position:relative;
  left:5000px;
  align-items: center;
  align-self: center;
}

#recentsearch{
  position:relative;
  top:82px;
  left:30px;
  background-color: grey;
  height: 30px;
  width: 469px;
  color: white;
  padding-left: 15px;

}

#recentsearch:hover{
  background-color: rgb(185, 185, 185);
}



.search-bar input {
  padding-left: 30px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  background:(120, 166, 203);
}
#h5{
  position: relative;
  left:500px;
  bottom: 50px;
}

</style>