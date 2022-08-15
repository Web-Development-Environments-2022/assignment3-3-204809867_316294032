<template>
  <div id="app">
     <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="#">Dar&IdoTouch</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
        </button>
         <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
                <li class="nav-item">
                    <router-link :to="{ name: 'main' }" class="nav-link">Home</router-link>
                </li>
                <li class="nav-item">
                  <router-link :to="{ name: 'search' }" class="nav-link">Search</router-link>
                </li>
                <li class="nav-item">
                  <router-link :to="{ name: 'about' }" class="nav-link">About</router-link>
                </li>

                <span v-if="$root.store.username">
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" @click="dropdowntoggle" >My Private Area</a>
                        <div class="dropdown-menu" v-bind:class="{ show: flagShow }" >
                            <router-link :to="{ name: 'favorite' }" class="dropdown-item">My Favorites</router-link>
                            <router-link :to="{ name: 'personal' }" class="dropdown-item">My Personal Recipes</router-link>
                            <!-- <router-link :to="{ name: 'family' }" class="dropdown-item">My Family</router-link> -->
                        </div>
                    </li>
                </span>
            </ul>
            <span v-if="!$root.store.username">Connected as <span style="color:gold;letter-spacing: 2px;">Guest</span>
                <button class="btn btn-outline" @click="$router.push('register')">Register</button>
                <button class="btn btn-outline" @click="$router.push('login')"> Login</button>
            </span>
            <span v-else> Connected as <span style="color:gold;letter-spacing: 2px;"> {{ $root.store.username }}</span>
                <button class="btn btn-outline-danger" @click="Logout">Logout</button>
            </span>
        </div>
</nav>
    <router-view/>
  </div>
</template>

<script>
export default {
    name: "App",
      data(){
    return{
        flagShow:false
    }; 
  },
  methods: {
    async Logout() {
      try {
        const response = await this.axios.post(
          //http://localhost:3000/Logout
          this.$root.store.server_domain + "/Logout", {withCredentials: true},
        );
        this.$root.store.logout();
        console.log("this is log out in server");
        console.log(response);

      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }

      this.$root.toast("Logout", "User logged out successfully", "success");
      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
        dropdowntoggle(){
      this.flagShow = !this.flagShow;
    },
    CreateRecipe() {
            console.log("inside the create recipe");
            this.$root.store.flagCreateRecipe = true;
            console.log(this.$root.store.flagCreateRecipe);
        }
  }};

</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  // background-image: url(assets/logo/background2.jpg);
  // background-color: #f9f6e6;
  font-family: system-ui;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  height: 100%;
  width: 100%;
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
