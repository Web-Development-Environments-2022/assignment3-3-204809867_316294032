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
              <div><img id="time-icon" src="../assets/logo/time-icon.png">
              Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>
                <img id="like-icon" src="../assets/logo/like.png">
                Likes: {{ recipe.popularity }} likes</div>
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
                response = await this.axios.get(this.$root.store.server_domain + "/recipes/" + text, { withCredentials: true}
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

            this.recipe = _recipe;
            this.isFavorite = _recipe.flagInFavorite;
            this.isLastseen = _recipe.flagInLastSeen;
            this.isVegan = _recipe.vegan;
            this.isVegetarian = _recipe.vegetarian;
            this.isGlutenfree = _recipe.glutenFree;
            //update the last seen recipe
            if (this.$root.store.username) {
                try {
                    const response = await this.axios.post(this.$root.store.server_domain + "/users/lastSeenRecipe", {
                        recipeId: this.$route.params.recipeId
                    }, { withCredentials: true });
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


#time-icon{
  width: 23px;
  height: 23px;
	margin-bottom: 3px;
	margin-top: -1px;

}

#like-icon{
  width: 22px;
  height: 22px;
	margin-top: -9px;
}

.recipe-body{
  border-style: double;
  border-width: 5px 9px; 
  padding: 10px;
}

</style>
