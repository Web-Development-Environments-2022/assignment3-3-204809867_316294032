<template>
  <div class="container">
    <h1 class="title">Search</h1>

    <b-form @submit.prevent="onSearching">
      <b-form-group id="input-group-Search" label="" label-for="Search">
        <b-form-input id="Search" style="width:450px" v-model="$v.form.searchQuery.$model" type="text"
          :state="validateState('searchQuery')" placeholder="Search recipes..."></b-form-input>
        <pre v-if="this.last_search && keyID!=0">your last search is:{{ last_search }}</pre>

        <b-form-invalid-feedback>
          Search query is requested.
        </b-form-invalid-feedback>
      </b-form-group>

      <div id="filtering" style="display:flex">
        <b-form-group id="input-group-cuisine" label="cuisine:" label-for="cuisine">
          <b-form-select id="cuisine" v-model="form.cuisine" :options="cuisines" style="width:auto"></b-form-select>
        </b-form-group>
        <b-form-group id="input-group-diet" label="diet:" label-for="diet">
          <b-form-select id="diet" v-model="form.diet" :options="diets" style="width:auto"></b-form-select>
        </b-form-group>
        <b-form-group id="input-group-into" label="intolerances:" label-for="intolerances">
          <b-form-select id="intolerances" v-model="form.Intolerances" :options="Intolerances" style="width:auto">
          </b-form-select>
        </b-form-group>
      </div>

      <div id="selected" class="selected">
        <b-form-group label="" v-slot="{ ariaDescribedby }">
          <b-form-radio class="selected" v-model="form.numberRecipes" name="some-radios" value="5">5</b-form-radio>

          <b-form-radio class="selected" v-model="form.numberRecipes" :aria-describedby="ariaDescribedby"
            name="some-radios" value="10">10
          </b-form-radio>
          <b-form-radio class="selected" v-model="form.numberRecipes" :aria-describedby="ariaDescribedby"
            name="some-radios" value="15">15
          </b-form-radio>
          <b-form-text> You can choose how many results would you like to be returned. Note!
            The default is 5. </b-form-text>
        </b-form-group>
      </div>

      <br>
      <b-form-group id="input-group-sortBy" class="sortBy" label="sort by:" label-for="sortBy">
        <b-form-select id="sortBy" class="sortBy" v-model="form.sortBy" :options="sortByOptions" style="width:auto">
        </b-form-select>
      </b-form-group>

      <br>
      <b-button type="submit" variant="primary" style="width:100px;display:block;" class="mx-auto w-100">search
      </b-button>
<br/>
    </b-form>

  <!--when click on Search the flag replace to true  -->
    <div v-if="flag"> 
      <b-alert v-if="results.length == 0" dismissible show> no results found for: {{ $v.form.searchQuery.$model }}</b-alert>
      <!-- go to SearchResults component: -->
      <SearchResults v-else :keyID="keyID" title="Results" :results="results"></SearchResults>
    </div>

  </div>
</template>


<script>
import cuisines from "../assets/cuisines";
import diets from "../assets/diets";
import Intolerances from "../assets/Intolerances";
import { required } from "vuelidate/lib/validators";
import SearchResults from "../components/SearchResults.vue";

export default {
  name: "Search",
  data() {
    return {
      form: {
        searchQuery: "",
        numberRecipes: 5,
        cuisine: "",
        diet: "",
        Intolerances: "",
        sortBy: "",
        

      },
      results: [],
      last_search: "", 
      cuisines: [{ value: "", text: "None" }],
      diets: [{ value: "", text: "None" }],
      Intolerances: [{ value: "", text: "None" }],
      flag: false,
      keyID: 0,
      sortByOptions: [{ value: "", text: "None" },
      { value: "popularity", text: "Popularity" },
      { value: "readyInMinutes", text: "Preparation" }]


    };
  },
  validations: {
    form: {
      searchQuery: {
        required
      }
    }
  },

  mounted(){
    console.log("in mounted");
    this.cuisines.push(...cuisines);
    this.diets.push(...diets);
    this.Intolerances.push(...Intolerances);
    
  },
  created() {
    console.log("in created");
    this.getMyLastSearch();
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      try {
        //with filtering cuisine/diet/intolerances and number of results.
        var myQuery = "/recipes/search?query=" + this.form.searchQuery + "&num=" + this.form.numberRecipes + "&cuisine=" + this.form.cuisine + "&diet=" + this.form.diet + "&intolerances=" + this.form.Intolerances;
        console.log(this.$root.store.server_domain + myQuery)
        const response = await this.axios.get(
          //http://localhost:3000/recipes/search?query=pizza
          this.$root.store.server_domain + myQuery, { withCredentials: true});

        const recipes = response.data; //results
        this.results = [];
        this.flag = false;

        this.results.push(...recipes);

        switch (this.form.sortBy) {
          case "popularity":
            console.log("Popularity");
            this.sortByPopularity();
            break;
          case "readyInMinutes": // readyInMinutes
            console.log("Preparation");
            this.sortByPreparation();
            break;
          default: //None
            console.log("None filtering - return like it is");
            break;
        }

        console.log(this.results);
        this.getMyLastSearch();

        this.flag = true;
        this.keyID += 1;

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
    },

    async sortByPopularity() { // sort Decending 
      this.results.sort(function (a, b) {
        if (a.popularity < b.popularity) {
          return 1;
        }
        if (a.popularity > b.popularity) {
          return -1;
        }
        return 0;
      });

    },
    async sortByPreparation() { // sort accecending 
      this.results.sort(function (a, b) {
        if (a.readyInMinutes < b.readyInMinutes) {
          return -1;
        }
        if (a.readyInMinutes > b.readyInMinutes) {
          return 1;
        }
        return 0;
      });

    },
    async getMyLastSearch() {
      try {
        if (this.$root.store.username) { //this will prevent error 401 - because some user has to be logged !!

            const response = await this.axios.get(
              this.$root.store.server_domain + "/users/lastSearch", { withCredentials: true});

            console.log("this is the response");
            console.log(response);

            console.log("before get lastSearch from the server:");
            console.log(this.last_search);
            console.log("After -> the response = lastSearch:");
            console.log(response);

            //update the last_search of the conacted user ! 
            this.last_search = response.data;

          }
        

      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.last_search ="";
      }
    },

    onReset() {
      this.form = {
        searchQuery: "",

      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  },
  components: { SearchResults }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
  font-family: system-ui;
}

.selected {
  display: inline-block;
  margin-right: 30px;
}

.sortBy {
  display: inline-block;
}
</style>
