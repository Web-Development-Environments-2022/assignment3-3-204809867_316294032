<template>
  <b-container>
    <h3>
      {{ title }}:
      <!-- {{ results }}: -->
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "SearchResults",
  components: {
    RecipePreview
  },
  props: {
    title: {  
      type: String,
      required: true
    },
    results:{
        type: Array,
        required: true
    },
    keyID:{
      type:Number,
      required: false
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.showRecipesResults();
  },
  watch:{
    keyID: function(){
      this.showRecipesResults()
    }
  },
  methods: {
    async showRecipesResults() {
      try {
        const recipes = this.results;
        console.log(recipes);
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
