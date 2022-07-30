<template>
  <div id="app">
    <img id="sim" src="./assets/sim.jpeg">  
    <div id="nav">
      <router-link  :to="{ name: 'main' }">     Simot Recipes     </router-link>   
      <router-link :to="{ name: 'search' }">     Search      </router-link>
      <router-link :to="{ name: 'about' }">     About      </router-link>
      <span v-if="!$root.store.username">
        <router-link :to="{ name: 'register' }">     Register     </router-link>   
        <router-link :to="{ name: 'login' }">     Login     </router-link>
      </span>
      <span v-else>
        <button @click="Logout" id="logout">     Logout     </button>   
        <b-dropdown style=" position: absolute;right:100px;" id="mydrop" text="Personal">
          <b-dropdown-item> <router-link :to="{ name: 'favorites' }">Favorites</router-link></b-dropdown-item>
          <b-dropdown-item><router-link :to="{ name: 'myrecipes' }">My recipes</router-link></b-dropdown-item>
          <b-dropdown-item><router-link :to="{ name: 'family' }">Family recipes</router-link></b-dropdown-item>
        </b-dropdown>
      </span>
         <!-- Trigger/Open The Modal -->
<button id="myBtn" @click="openModal">Create New Recipe</button>

<!-- The Modal -->
<div id="myModal" class="modal">

  <!-- Modal content -->
  <div class="modal-content">
    <span class="close" @click="closeModal">&times;</span>
    <h3>New Recipe Details:</h3>
    <b-form @submit.prevent="onSubmit">
      <b-form-group
        id="input-group-name"
        label="Title (name):"
        label-cols-sm="1"
        label-for="Recipename"
      >
       <b-form-input class="input" id="title" type="text" v-model="$v.form.title.$model" :state="validateState('title')">
    </b-form-input>
       <b-form-invalid-feedback v-if="!$v.form.title.required">
          Title is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.title.length">
          Title length is too long
        </b-form-invalid-feedback>
       </b-form-group>
       <b-form-group
        id="input-group-readyIn"
        label="Preparation time (minutes):"
        label-for="readyIn"
        label-cols-sm="2"
      >
       <b-form-input id="readyInMinutes" class="short" type="number" v-model="$v.form.readyInMinutes.$model" :state="validateState('readyInMinutes')"></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">
          Preparation time is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.readyInMinutes.length">
          Preparation time is too long
        </b-form-invalid-feedback>
       </b-form-group>
       <b-form-group
        id="input-group-image"
        label="Image:"
        label-cols-sm="1"
        label-for="Recipename"
      >
       <b-form-input class="input" id="image" type="text"></b-form-input>
       </b-form-group>
       <b-form-group
        id="input-group-instructions"
        label="Ingredients:"
        label-for="Ingredients"
        label-cols-sm="1"

      >
       <b-form-input id="ingredients" type="text" v-model="$v.form.ingredients.$model" :state="validateState('ingredients')"></b-form-input>
        <b-form-invalid-feedback>
          Ingredients are required
        </b-form-invalid-feedback>
       </b-form-group>

      <b-form-group
        id="input-group-instructions"
        label="Instructions:"
        label-for="Instructions"
        label-cols-sm="1"
      >     
       <b-form-input id="instructions" type="text" v-model="$v.form.instructions.$model" :state="validateState('instructions')"></b-form-input>
       <b-form-invalid-feedback>
          Instructions are required
        </b-form-invalid-feedback>
       </b-form-group>
        <b-form-group
        id="input-group-Servings"
        label="Servings:"
        label-for="Servings"
        label-cols-sm="1"
      >
       <b-form-input id="servings" class="short" type="number" v-model="$v.form.servings.$model" :state="validateState('servings')"></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.servings.required">
          Servings are required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.servings.length">
          servings number is too long
        </b-form-invalid-feedback>
       </b-form-group>
      <input type="checkbox" id="gluttenFree" value="GluttenFree" v-model="selected">
      <label for="GluttenFree">Glutten Free </label>
      &nbsp;&nbsp;
      <input type="checkbox" id="vegan" value="Vegan" v-model="selected">
      <label for="Vegan"> Vegan &nbsp;   </label>
      &nbsp;&nbsp;
      <input type="checkbox" id="vegeterian" value="Vegeterian" v-model="selected">
      <label for="Vegeterian">Vegeterian</label>
      <b-button
        type="submit"
        variant="primary"
        style="width:100px;display:block;"
        class="mx-auto w-100"
        >Submit</b-button
      >
     </b-form>
     <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      creating recipe failed: {{ form.submitError }}
    </b-alert> 
  </div>
 

</div>
    <span v-if="!$root.store.username">
    Hello guest!
    </span>
    <span v-else>
    Hello {{ $root.store.username }}!
    </span>  
    </div>
    <router-view />
  </div>
</template>

<script>
import {
  required,
  maxLength,
} from "vuelidate/lib/validators";

export default {
  name: "App",
      data() {
      return {
        form: {
          title: "",
          readyInMinutes: "",
          ingredients: "",
          instructions: "",
          instructions: "",
          servings: "",
          vegan: false,
          vegetarian: false,
          glutenFree: false,
          submitError: undefined
      },
      errors: [],
      validated: false,
      selected: [], // Must be an array reference!
      }
    },
  validations: {
    form: {
      title: {
        required,
        length: (u) => maxLength(40)(u),
      },
      readyInMinutes: {
        required,
        length: (u) => maxLength(5)(u),
      },
      ingredients: {
        required,
      },
      instructions: {
        required,
      },
      servings: {
        required,
        length: (u) => maxLength(5)(u),
      }
    }
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    onSubmit() {
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      this.newRecipe();
    },

    async newRecipe() {
       try {
        const response = await this.axios.post(
          "http://localhost:3000/users/myrecipe",
          {
            title: this.form.title,
            readyInMinutes: this.form.readyInMinutes,
            image: this.form.image,
            vegan: this.form.vegan ? 1 : 0,
            vegetarian: this.form.vegetarian ? 1 : 0,
            glutenFree: this.form.glutenFree ? 1 : 0,
            ingredients: this.form.ingredients,
            instructions: this.form.instructions,
            servings: this.form.servings,
          }
        );
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
  openModal(){
      // Get the modal
 var modal = document.getElementById("myModal");


// When the user clicks on the button, open the modal

  modal.style.display = "block";
},
closeModal(){
      // Get the modal
 var modal = document.getElementById("myModal");

// When the user clicks on the button, open the modal

   modal.style.display = "none";
},

    }

  };
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#myBtn{
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background: none;
  color: #2c3e50;
  border: 0ch;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background: rgb(202, 222, 229);
}

#mydrop{
    background: none;
}

#nav {
  background-color: rgb(120, 166, 203);
}

#nav a, #logout, #myBtn {
  font-family: 'Raleway',sans-serif;
  font-weight: 800;
  font-size: 20px;
  color: #2c3e50;
}

#nav a:hover, #logout:hover, #myBtn:hover{
  font-size: 25px;
  text-decoration: underline;
}

#logout {
  background-color: Transparent;
  background-repeat:no-repeat;
  border: none;
}

#nav a.router-link-exact-active {
  color: #ffffff;
  font-size: 25px;
  background-color: rgb(83, 114, 139);
  padding: 11px;
}

#sim {
  margin-left: auto;
  margin-right: auto;
}
.input{ 
  max-width: 200px;
  background-color: #2c3e50;
}
.short{ 
  max-width:100px;
  background-color: #2c3e50;
}

/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 50%; /* Full width */
  height: 50%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  top:-120px;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 50%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

</style>