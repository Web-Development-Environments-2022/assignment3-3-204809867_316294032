<template>
  <div class="container">
    <h1 class="title">Search</h1>
    <b-form @submit.prevent="onSearching">
      <!-- label = Search: -->
      <b-form-group id="input-group-Search" label="" label-for="Search">
        <b-form-input id="Search" style="width:450px" v-model="$v.form.searchQuery.$model" type="text"
          :state="validateState('searchQuery')" placeholder="Search recipes..."></b-form-input>

        <b-form-invalid-feedback>
          Search query is requested.
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- <div id="filtering" style="display:flex"> -->
      <div id="filtering">
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


      <!-- <div id="selectedr">
        <label style="padding:5px"><input type="radio" id="num5" value="5" v-model="form.numberRecipes"
            aria-describedby="defult-choose-help-block">5</label>
        <label style="padding:5px"><input type="radio" id="num10" value="10" v-model="form.numberRecipes">10</label>
        <label style="padding:5px"><input type="radio" id="num15" value="15" v-model="form.numberRecipes">15</label>
      </div>
      <b-form-text id="defult-choose-help-block"> You can choose how many results would you like to be returned. Note!
        default is 5. </b-form-text>
      <br> -->
      <b-form-group label="" v-slot="{ ariaDescribedby }">
        <b-form-radio v-model="form.numberRecipes" :aria-describedby="defult-choose-help-block" name="some-radios" value="5">5</b-form-radio>
        <b-form-radio v-model="form.numberRecipes" :aria-describedby="ariaDescribedby" name="some-radios" value="10">10</b-form-radio>
        <b-form-radio v-model="form.numberRecipes" :aria-describedby="ariaDescribedby" name="some-radios" value="15">15</b-form-radio>
      </b-form-group>
      <b-form-text id="defult-choose-help-block"> You can choose how many results would you like to be returned. Note!
        default is 5. </b-form-text>
      <!-- <div class="mt-3">Selected: <strong>{{ form.numberRecipes }}</strong></div> -->
      <br>
      <b-button type="submit" variant="primary" style="width:100px;display:block;" class="mx-auto w-100">search
      </b-button>
    </b-form>


    <!-- this part will be for no results in v-else!!! -->
    <b-alert class="mt-2" v-if="form.submitError" variant="warning" dismissible show>
      no results found for: {{ $v.form.searchQuery.$model }}
    </b-alert>
    <!-- <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card> -->

    <div v-if="flag">
      <b-alert v-if="results.length == 0" dismissible show> no results found for: {{ $v.form.searchQuery.$model }}
      </b-alert>
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
        searchQuery: localStorage.searchQuery || "",
        numberRecipes: 5,
        cuisine: "",
        diet: "",
        Intolerances: "",
        submitError: undefined, //check if need it.
      },
      results: [],
      last_search: "",
      cuisines: [{ value: "", text: "None" }],
      diets: [{ value: "", text: "None" }],
      Intolerances: [{ value: "", text: "None" }],
      flag: false,
      keyID: 0,
      
    };
  },
  validations: {
    form: {
      searchQuery: {
        required
      }
    }
  },
  mounted() {
    // console.log("mounted");
    this.cuisines.push(...cuisines);
    this.diets.push(...diets);
    this.Intolerances.push(...Intolerances);
    //console.log($v);
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
        this.searchQuery = this.form.searchQuery; //maybe dont nedd
        var myQuery = "/recipes/search?query=" + this.form.searchQuery + "&num=" + this.form.numberRecipes + "&cuisine=" + this.form.cuisine + "&diet=" + this.form.diet + "&intolerances=" + this.form.Intolerances;
      
        //TODO: need to check with filtering too. cuisine/diet/intolerances and number of results.
        console.log(this.$root.store.server_domain + myQuery)
        const response = await this.axios.get(
          //http://localhost:3000/recipes/search?query=pizza
          this.$root.store.server_domain + myQuery, { withCredentials: true, credentials: "include" });
        console.log(response);
        const recipes = response.data; //results
        this.results = [];
        this.flag = false;

        this.results.push(...recipes);
        console.log(this.results);
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
}
</style>
