<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <iconBar class="iconbar" :isFavorite="isFavorite" :isLastseen="isLastseen" :isVegetarian="isVegetarian" :isVegan="isVegan" :isGlutenfree="isGlutenfree" ></iconBar>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.popularity }} likes</div>
            </div>
            Ingredients:
            <br>
            {{recipe.IngredientsList}}
          </div>
          <div class="wrapped">
            Instructions:
            <br>
            {{recipe.Instructions}}
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import iconBar from '../components/iconBar.vue';
export default {
    data() {
        return {
            recipe: null,
            isFavorite:false,
            isLastseen:false,
            isVegan:false,
            isVegetarian:false,
            isGlutenfree:false
        };
    },
    async created() {
        try {
            let response;
            try {
                let num = this.$route.params.recipeId;
                let text = num.toString();
                response = await this.axios.get(this.$root.store.server_domain + "/users/myrecipe/" + text, { withCredentials: true}
                //"http://localhost:3000/recipes/" + text,{withCredentials: true}
                );
                if (response.status !== 200)
                    this.$router.replace("/NotFound");
            }
            catch (error) {
                console.log("error.response.status", error.response.status);
                this.$router.replace("/NotFound");
                return;
            }
            let {title,readyInMinutes,image,popularity,vegan,vegetarian,glutenFree,servings,Instructions,IngredientsList} = response.data[0];
            console.log(response.data[0]);
            let _recipe = {title,readyInMinutes,image,popularity,vegan,vegetarian,glutenFree,servings,Instructions,IngredientsList};
            console.log(_recipe);
            this.recipe = _recipe;
            this.isFavorite = true;
            this.isLastseen = true;
            this.isVegan = _recipe.vegan;
            this.isVegetarian = _recipe.vegetarian;
            this.isGlutenfree = _recipe.glutenFree;
        }
        catch (error) {
            console.log(error);
        }
    },
    components: { iconBar }
};
</script>
<style scoped>
.wrapper {
  display: flex;
}

.wrapped {
  width: 50%;
}

.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}

</style>
