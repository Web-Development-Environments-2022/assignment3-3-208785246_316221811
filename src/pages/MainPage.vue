<template>
  <div class="container">
     <div class="container1"></div>
    <div class="col">
    <RecipePreviewList id="randomRecipes" title="Explore this recipes" class="RandomRecipes center" />
    </div>
    <div class="col">
      <login-page v-if="!$root.store.username" id="login"></login-page>
    </div>
    <div class="col">
    <RecipeViewedPreviewList id="viewed" v-if="$root.store.username" title="Last Viewed Recipes" ></RecipeViewedPreviewList>
    </div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import RecipeViewedPreviewList from "../components/RecipeViewedPreviewList";
import LoginPage from './LoginPage.vue';
export default {
  components: {
    RecipePreviewList,
    RecipeViewedPreviewList,
    LoginPage
  },
  data(){
        return{logo: '\src\assets\simot.jpeg'
        }
        

  },
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;

}

.container1{
   display: table;
    width: 150%; /*Optional*/
    table-layout: fixed; /*Optional*/
   // border-spacing: 10px; /*Optional*/
}
.col{
display: table-cell;
width: 800px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
#login{
  width: 400px;
  position: relative;
  bottom: -20px;
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}


</style>
