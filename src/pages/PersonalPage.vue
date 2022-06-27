<template>
  <div class="container">
    <h1 class="title">Personal recipe:</h1>
    <div>THIS IS A MODAL FOR ADDING PERSONAL RECIPES</div>

    <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this page</router-link>
    <!-- {{ !$root.store.username }} this is false whan we longged in -->

    <div v-if="flagResponse">
      <PersonalResults title="Results" :results="results"></PersonalResults>
    </div>


  </div>
</template>

<script>
import PersonalResults from "../components/PersonalResults";
export default {
  name: "personal",
  data() {
    return {
      results:[],
      flagResponse: false
    };
  },
  async created(){
    //if the user is connected
    if (this.$root.store.username) {
        try {
            //console.log("IM IN THE IF");
            const response = await this.axios.get(this.$root.store.server_domain + "/users/myrecipe", { withCredentials: true, credentials: "include" });
            this.results = response.data
            this.flagResponse = true
        }
        catch (err) {
            console.log("error.response.status", error.response.status);
        }
    }

  },
  components: { PersonalResults }
};
</script>


<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
