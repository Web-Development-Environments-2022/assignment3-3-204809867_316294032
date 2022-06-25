<template>
  <div class="container">
    <h1 class="title">Search</h1>
    <b-form @submit.prevent="onSearching">
      <b-form-group id="input-group-Username" label-cols-sm="3" label="Search:" label-for="Search">
        <b-form-input id="Search" v-model="$v.form.searchQuery.$model" type="text" :state="validateState('searchQuery')"
          placeholder="Search recipes..."></b-form-input>
        <b-form-invalid-feedback>
          Search query is requested.
        </b-form-invalid-feedback>
      </b-form-group>

      <b-button type="submit" variant="primary" style="width:100px;display:block;" class="mx-auto w-100">click to search
      </b-button>
    </b-form>
    <b-alert class="mt-2" v-if="form.submitError" variant="warning" dismissible show>
      no results found for: {{ $v.form.searchQuery.$model }}
    </b-alert>
    <!-- <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card> -->

    <!-- <span v-if= "results.length != 0">
      go to SearchResults component:
    </span>
    <span v-else>
     username connect this part show his name 'Dar273'
      {{ $root.store.username }}: <button @click="Logout">Logout</button>|
    </span> -->
    <div v-if="flag">
      <SearchResults title="Results" :results="results"></SearchResults>
    </div>


  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";
import SearchResults from "../components/SearchResults.vue";
export default {
  name: "Search",
  data() {
    return {
      form: {
        searchQuery: localStorage.searchQuery || "",
        results: [],
        last_search: "",
        numberRecipesTo_show: 5,
        submitError: undefined,
      },
      flag: false
    };
  },
  validations: {
    form: {
      searchQuery: {
        required
      }
    }
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      try {
        //console.log(this.form.searchQuery);
        //console.log("myQuery");
        var myQuery = "/recipes/search?query=" + this.form.searchQuery;
        //TODO: need to check with filtering too. cuisine/diet/intolerances and number of results.
        //console.log(myQuery);

        const response = await this.axios.get(
          //http://localhost:3000/recipes/search?query=pizza
          this.$root.store.server_domain + myQuery, { withCredentials: true, credentials: "include" });
        console.log(response);
        const recipes = response.data; //results
        this.results = [];
        this.flag = false;

        this.results.push(...recipes);
        console.log(this.results);
        console.log("this.results - in father");
        this.flag = true;


      }
      catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onSearching() {
      console.log("start to search...");
      this.form.submitError = undefined;
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      console.log("Search method go");
      this.Search();
    }
  },
  components: { SearchResults }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
