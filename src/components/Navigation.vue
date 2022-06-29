<template>
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
      </span>
    </div> -->


    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="#">Dar&idoTouch</a>
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
                <!-- <li class="nav-item dropdown">
                  <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Dropdown</a>
                  <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                    <a class="dropdown-item" href="#">Action</a>
                    <a class="dropdown-item" href="#">Another action</a>
                    <div class="dropdown-divider"></div>
                    <a class="dropdown-item" href="#">Something else here</a>
                  </div>
                </li> -->
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
            <!-- <form class="form-inline my-2 my-lg-0">
                <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
                <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
            </form> -->
        </div>
</nav>
</template>


<script>
export default {
  name: "Navigation",
  data(){
    return{
        flagShow:false
    };
    
  },
  methods: {
    async Logout() {
      this.$root.store.logout();
      try {
        const response = await this.axios.post(
          //http://localhost:3000/Logout
          this.$root.store.server_domain + "/Logout"
        );
        console.log(response);
        this.$root.loggedIn = false;
        console.log(this.$root.store.logout);
      } catch (err) {
        console("problem herrrrre");
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
    }
  }};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";


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