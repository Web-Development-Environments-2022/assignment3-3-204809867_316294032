<template>
  <div class="container">
    <h1 class="title">Personal recipe:</h1>

    <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this page</router-link>
    <!-- {{ !$root.store.username }} this is false whan we longged in -->

    <!-- button : create recipe '+' -->
    <b-button v-b-modal.mymodal>+ create recipe</b-button> 
    <b-modal modal-class="my-class" id="mymodal" ref="modal" title="create recipe" size="xl">
        <div>
            <b-form @submit="onSubmit" @reset="onReset" v-if="show">

            <b-form-group id="input-group-2" label="Your recipe's title:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.title" placeholder="ex: Gefilte Fish" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's time to cook:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.readyInMinutes" placeholder="ex: 15" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's image:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.image" placeholder="ex: http://www.dar_abu_told_me_she_loves_gefilte_fishh.com/picture" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's popularity:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.popularity" placeholder="ex: 2" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's vegan:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.vegan" placeholder="ex: false" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's vegetarian:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.vegetarian" placeholder="ex: true" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's glutenFree:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.glutenFree" placeholder="ex: false" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's servings:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.servings" placeholder="ex: 15" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's Instructions:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.Instructions" placeholder="ex: take the karpion, try to cook it somehow, or simply go ask your savta, she probably knows" required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Your recipe's IngredientsList:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.IngredientsList" placeholder="ex: karpion, carrots, salt,..." required></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-3" label="Vegan:" label-for="input-3">
              <b-form-select id="input-3" v-model="form.vegan" :options="foodsvegan" required></b-form-select>
            </b-form-group>

            <b-form-group id="input-group-3" label="Vegetarian:" label-for="input-3">
              <b-form-select id="input-3" v-model="form.vegetarian" :options="foodsvegetarian" required></b-form-select>
            </b-form-group>

            <b-form-group id="input-group-3" label="Gluten Free:" label-for="input-3">
              <b-form-select id="input-3" v-model="form.glutenFree" :options="foodsglutenFree" required></b-form-select>
            </b-form-group>

            <b-button type="submit" variant="primary">Submit</b-button>
            <b-button type="reset" variant="danger">Reset</b-button>
            </b-form>

         </div>
        </b-modal>
    <div v-if="flagResponse">
      <PersonalResults title="Results" :results="results"></PersonalResults>
    </div>



  </div>
</template>

<script>
import PersonalResults from "../components/PersonalResults";
import { BButton } from "bootstrap-vue";
import { required, } from "vuelidate/lib/validators";

export default {
  name: "personal",
  data() {
    return {
        form: {
                title: "",
                readyInMinutes: "",
                image: "",
                popularity: "",
                vegan: "",
                vegetarian:"",
                glutenFree:"",
                servings:"",
                Instructions:"",
                IngredientsList:"",
            },
        foodsvegan: [{ text: 'Select One', value: null }, 'true', 'false'],
        foodsvegetarian: [{ text: 'Select One', value: null }, 'true', 'false'],
        foodsglutenFree: [{ text: 'Select One', value: null }, 'true', 'false'],
        show: true,
        results:[],
        showModal: false,
        flagResponse: false
    };
  },
  validations: {
        form: {
            title: {
                required,
            },
            readyInMinutes: {
                required,
            },
            image: {
                required,
            },
            popularity: {
                required,
            },
            vegan: {
                required,
            },
            vegetarian: {
                required,
            },
            glutenFree: {
                required,
            },
            servings: {
                required,
            },
            Instructions: {
                required,
            },
            IngredientsList: {
                required,
            },
        }
    },
  async created(){
    //if the user is connected
    if (this.$root.store.username) {
        try {
            const response = await this.axios.get(this.$root.store.server_domain + "/users/myrecipe", { withCredentials: true,});
            this.results = response.data
            this.flagResponse = true
        }
        catch (err) {
            console.log("error.response.status", error.response.status);
        }
    }

  },
  methods: {
      async createMyNewRecipe(){
          try {
            const response = await this.axios.post(this.$root.store.server_domain + "/users/myrecipe",
            {
              title: this.form.title,
              readyInMinutes: this.form.readyInMinutes,
              image: this.form.image,
              popularity:this.form.popularity,
              vegan: this.form.vegan,
              vegetarian: this.form.vegetarian,
              glutenFree: this.form.glutenFree,
              servings:this.form.servings,
              Instructions: this.form.Instructions,
              IngredientsList: this.form.IngredientsList,
            },{ withCredentials: true }

            );
            console.log(response);
          } catch (err) {
            console.log(err.response);
            this.form.submitError = err.response.data.message;
          }


      },
      onSubmit(event) {
        event.preventDefault()
        console.log(this.form)
        this.createMyNewRecipe()
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
       this.form = {
                title: "",
                readyInMinutes: "",
                image: "",
                popularity: "",
                vegan: "",
                vegetarian:"",
                glutenFree:"",
                servings:"",
                Instructions:"",
                IngredientsList:"",
            };
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    
    },
  
  components: { PersonalResults, BButton }
};
</script>


<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
