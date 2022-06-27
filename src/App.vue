<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link :to="{ name: 'main' }">Main</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <router-link :to="{ name: 'favorite' }">Favorite</router-link>|
      <router-link :to="{ name: 'personal' }">Personal</router-link>|
      <router-link :to="{ name: 'about' }">About</router-link>|
      <span v-if="!$root.store.username">
        hello guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
        <b-button v-b-modal.mymodal>create recipe</b-button>
        <b-modal modal-class="my-class" id="mymodal" ref="modal" title="create recipe" size="xl">this is a test</b-modal>
      </span>
    </div> -->

    <Navigation></Navigation>
    <router-view />
  </div>
</template>

<script>
import Navigation from "./components/Navigation.vue";
export default {
    name: "App",
    methods: {
        Logout() {
            this.$root.store.logout();
            this.$root.toast("Logout", "User logged out successfully", "success");
            this.$router.push("/").catch(() => {
                this.$forceUpdate();
            });
        },
        CreateRecipe() {
            console.log("inside the create recipe");
            this.$root.store.flagCreateRecipe = true;
            console.log(this.$root.store.flagCreateRecipe);
        }
    },
    components: { Navigation }
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
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
