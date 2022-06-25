<template>
  <div class="container">
    <h1 class="title">Search by recipe ID</h1>
    <b-form @submit.prevent="onSearch">
      <b-form-group
        id="input-group-searchId"
        label-cols-sm="3"
        label="searchId:"
        label-for="searchId"
      >
        <b-form-input
          id="searchId"
          v-model="$v.form.searchId.$model"
          type="number"
          min="0"
          oninput="validity.valid||(value='')"
          step="1"
          :state="validateState('searchId')"
        ></b-form-input>
        <b-form-invalid-feedback>
          searchId is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-button
        type="submit"
        variant="primary"
        style="width:100px;display:block;"
        class="mx-auto w-100"
        >Search</b-button
      >

      <!-- <div class="mt-2">
        You don't know what to search?
        <router-link to="random"> Random a recepee here</router-link>
      </div> -->

    </b-form>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Search failed: {{ form.submitError }}
    </b-alert>
    <!-- <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card> -->
  </div>
</template>

<script>
import { type } from "os";
import { required } from "vuelidate/lib/validators";
export default {
  name: "Login",
  data() {
    return {
      form: {
        searchId: "",
        submitError: undefined
      }
    };
  },
  validations: {
    form: {
      searchId: {
        required
      }
    }
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    
    async Login() {
      try {
        
        const response = await this.axios.post(
          //// "https://test-for-3-2.herokuapp.com/user/Login",
          this.$root.store.server_domain +"/Login",
          //// "http://132.72.65.211:80/Login",
          //// "http://132.73.84.100:80/Login",
          // "http://localhost:3000/Login",
          {
            username: this.form.username,
            password: this.form.password
          }
        );
        //console.log(response);
        // this.$root.loggedIn = true;
        //console.log(this.$root.store.login);
        this.$root.store.login(this.form.username);
        this.$router.push("/");
      } catch (err) {
        //console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onSearch() {
      // console.log("login method called");
      this.form.submitError = undefined;
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      // console.log("login method go");

      this.Login();
    }
  }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
