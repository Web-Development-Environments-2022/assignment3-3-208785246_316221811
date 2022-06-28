<template>
  <div class="container">
     <div class="container1"></div>
    <div class="col">
    <RecipePreviewList id="randomRecipes" title="Explore this recipes" class="RandomRecipes center" />
    </div>
    <div class="col">
    <RecipeViewedPreviewList v-if="$root.store.username" title="Last Viewed Recipes" ></RecipeViewedPreviewList>
    <RecipePreviewList v-if="!$root.store.username" title="Last Viewed Recipes" 
    :class="{
        RandomRecipes: true,
        blur: !$root.store.username,
        center: true
      }"
      disabled
      ></RecipePreviewList>
    <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to view this</router-link>
    </div>
    <login-page v-if="!$root.store.username"></login-page>
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
    width: 100%; /*Optional*/
    table-layout: fixed; /*Optional*/
   // border-spacing: 10px; /*Optional*/
}
.col{
display: table-cell;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

</style>
