<template>
  <b-container>
    <h3>
      <!-- the title is 'Explore this recipes:' get parameters from his father MainPage-->
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
     <div v-if="call_type == 'random'">
        <b-button pill variant="outline-secondary" class="bbb" @click="updateRandomWith3More">More</b-button> 
      </div>
  </b-container>
 
  
  
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {  
      type: String,
      required: true
    },
    call_type: {  
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
      // flagiflag = false
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        let response;
        switch(this.$props.call_type)
        {
          case "random":
            // this.flagiflag = true
            response = await this.axios.get(
              this.$root.store.server_domain + "/recipes/random/3",
              // "https://test-for-3-2.herokuapp.com/recipes/random"
              //"http://localhost:3000/recipes/random/3",
            );
            break;
          case "lastWatch":
            response = await this.axios.get(
              this.$root.store.server_domain + "/users/lastSeenRecipe/3",{withCredentials: true, credentials: 'include'}
              //http://localhost:3000/users/lastSeenRecipe/:numb
            );
            break;
          default:
             response = await this.axios.get(
              this.$root.store.server_domain + "/recipes/random/3",
              // "https://test-for-3-2.herokuapp.com/recipes/random"
              //"http://localhost:3000/recipes/random/3",
            );
            break;


          
        }
        // console.log(response);
        const recipes = response.data;
        // console.log(recipes);
        this.recipes = [];
        this.recipes.push(...recipes);
        
      } catch (error) {
        console.log(error);
      }
    },
    async updateRandomWith3More(){
      let response;
      try{
        response = await this.axios.get(
              this.$root.store.server_domain + "/recipes/random/3",
              // "https://test-for-3-2.herokuapp.com/recipes/random"
              //"http://localhost:3000/recipes/random/3",
            );
      // console.log(response);
        const recipes = response.data;
        // console.log(recipes);
        this.recipes = [];
        this.recipes.push(...recipes);
        
      } catch (error) {
        console.log(error);
      }
      

    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
