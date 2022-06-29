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
export default {
  name: "App",
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

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

#nav a, #logout {
  font-family: 'Raleway',sans-serif;
  font-weight: 800;
  font-size: 20px;
  color: #2c3e50;
}

#nav a:hover, #logout:hover{
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

</style>
