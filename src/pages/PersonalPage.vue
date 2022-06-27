<template>
  <div class="container">
    <h1 class="title">Personal recipe:</h1>
    <!-- <div>THIS IS A MODAL FOR ADDING PERSONAL RECIPES</div> -->
    <!-- <button @click="showModal = true">Save</button> -->
    
    
    <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this page</router-link>
    <!-- {{ !$root.store.username }} this is false whan we longged in -->
    <b-button v-b-modal.mymodal>create recipe</b-button>
    <b-modal modal-class="my-class" id="mymodal" ref="modal" title="create recipe" size="xl">
        <div>
            <b-form @submit="onSubmit" @reset="onReset" v-if="show">

            <b-form-group id="input-group-2" label="Your recipe's title:" label-for="input-2">
                <b-form-input
                id="input-2"
                v-model="form.title"
                placeholder="Enter Title"
                required
                ></b-form-input>
            </b-form-group>

            <!-- <b-form-group id="input-group-3" label="Food:" label-for="input-3">
                <b-form-select
                id="input-3"
                v-model="form.food"
                :options="foods"
                required
                ></b-form-select>
            </b-form-group> -->

            <b-form-group id="input-group-4" v-slot="{ ariaDescribedby }">
                <b-form-checkbox-group
                v-model="form.checked"
                id="checkboxes-4"
                :aria-describedby="ariaDescribedby"
                >
                <b-form-checkbox value="true">Vegan</b-form-checkbox>
                <b-form-checkbox value="true" >Vegetarian</b-form-checkbox>
                <b-form-checkbox value="true" >GlutenFree</b-form-checkbox>
                </b-form-checkbox-group>
            </b-form-group>

            <b-button type="submit" variant="primary">Submit</b-button>
            <b-button type="reset" variant="danger">Reset</b-button>
            </b-form>
            <b-card class="mt-3" header="Form Data Result">
            <pre class="m-0">{{ form }}</pre>
            </b-card>
         </div>
        </b-modal>
    <div v-if="flagResponse">
      <PersonalResults title="Results" :results="results"></PersonalResults>
    </div>

    <!-- <CreateRecipeModal v-show="showModal" @close-modal="showModal = false" /> -->


  </div>
</template>

<script>
import PersonalResults from "../components/PersonalResults";
// import CreateRecipeModal from "../components/CreateRecipeModal";

export default {
  name: "personal",
  data() {
    return {
        form: {
            title: '',
            // food: null,
            checked: []
        },
        // foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
        show: true,
        results:[],
        showModal: false,
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
  methods: {
      onSubmit(event) {
        event.preventDefault()
        alert(JSON.stringify(this.form))
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
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
