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
              <!-- this in the page of recipe -->
            </div>
            Ingredients:
            <ul>
           
              <li v-for="(r, index) in recipe.extendedIngredients" :key="index + '_' + r.id">
                {{ r.original }}
              </li>
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <ol>
              <li v-for="s in recipe._instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
          </div>
          <!-- <div v-if="flagiflagInLastSeen">
            <h8>This recipee is in the last seen list !!</h8>
          </div>
          <div v-if="flagiflagInFavorite">
            <h8>This recipee is in the favorite list !!</h8>
          </div> -->
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
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
            // response = this.$route.params.response;
            //console.log(this.$route.params.recipeId)
            try {
                let num = this.$route.params.recipeId;
                let text = num.toString();
                response = await this.axios.get(this.$root.store.server_domain + "/recipes/" + text, { withCredentials: true, credentials: "include" }
                //"http://localhost:3000/recipes/" + text,{withCredentials: true, credentials: 'include'}
                );
                if (response.status !== 200)
                    this.$router.replace("/NotFound");
            }
            catch (error) {
                console.log("error.response.status", error.response.status);
                this.$router.replace("/NotFound");
                return;
            }
            let { analyzedInstructions, instructions, ingredients, vegan, vegetarian, glutenFree, extendedIngredients, popularity, readyInMinutes, image, title, flagInFavorite, flagInLastSeen } = response.data;
            console.log(response.data);
            let _instructions = analyzedInstructions
                .map((fstep) => {
                fstep.steps[0].step = fstep.name + fstep.steps[0].step;
                return fstep.steps;
            })
                .reduce((a, b) => [...a, ...b], []);
            let _recipe = {
                instructions,
                ingredients,
                vegan,
                vegetarian,
                glutenFree,
                _instructions,
                analyzedInstructions,
                extendedIngredients,
                popularity,
                readyInMinutes,
                image,
                title,
                flagInFavorite,
                flagInLastSeen
            };
            console.log(typeof(_recipe.analyzedInstructions))
            console.log(typeof(_recipe.extendedIngredients))

            //console.log(_recipe);
            this.recipe = _recipe;
            this.isFavorite = _recipe.flagInFavorite;
            this.isLastseen = _recipe.flagInLastSeen;
            this.isVegan = _recipe.vegan;
            this.isVegetarian = _recipe.vegetarian;
            this.isGlutenfree = _recipe.glutenFree;
            //update the last seen recipe
            if (this.$root.store.username) {
                try {
                    //console.log("IM IN THE IF");
                    const response = await this.axios.post(this.$root.store.server_domain + "/users/lastSeenRecipe", {
                        recipeId: this.$route.params.recipeId
                    }, { withCredentials: true, credentials: "include" });
                    //console.log("IM IN THE IF AFTER THE RESPONSE");
                    //console.log(response);
                }
                catch (err) {
                    console.log("error.response.status", error.response.status);
                }
            }
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

/* .recipe-header{

} */
</style>
