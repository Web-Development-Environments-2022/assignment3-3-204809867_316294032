<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <RecipePreviewPersonal class="recipePreviewPersonal" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
</template>

<script>
import RecipePreviewPersonal from "./RecipePreviewPersonal.vue";
export default {
  name: "PersonalResults",
  components: {
    RecipePreviewPersonal
  },
  props: {
    title: {  
      type: String,
      required: true
    },
    results:{
        type: Array,
        required: true

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
