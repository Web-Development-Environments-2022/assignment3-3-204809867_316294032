<template>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
    class="recipe-preview">

    <div class="recipe-body">
      <img v-if="image_load" :src="recipe.image" class="recipe-image" />
    </div>
    <div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title">
        {{ recipe.title }}
      </div>
      <ul class="recipe-overview">
        
        <li>
          <img id="time-icon" src="../assets/logo/time-icon.png">
          {{ recipe.readyInMinutes }} minutes</li>
        
        <li>{{ recipe.popularity }} <img id="like-icon" src="../assets/logo/like.png"></li>
        <li>{{recipe.servings}} servings</li>
        <!-- this in the main page! -->
      </ul>
      <ul class="recipe-overview">
        <!-- Vegan -->
        <li v-if="recipe.vegan">
        <!-- Vegan -->
          <img id="vegan-icon" src="../assets/logo/vegan.png">
        </li>
        
        <!-- Vegetarian -->
        <li v-if="recipe.vegetarian">
        <!-- Vegetarian -->
          <img id="vegetarian-icon" src="../assets/logo/vegetarian.png">
        </li>

        <!-- Gluten-Free -->
        <li v-if="recipe.glutenFree">
        <!-- Gluten-Free -->
          <img id="glutenFree-icon" src="../assets/logo/gluten-free.png">
        </li>

      </ul>
      <br/>
      <ul class="recipe-overview">
        <li v-if="recipe.flagInFavorite" >
          <img id="favorite-icon" src="../assets/logo/Favorite.jpg">
          In Favorite</li>
        <li v-if="recipe.flagInLastSeen" >Last-Seen</li>

      </ul>     
      <!-- <div v-if="recipe.flagInLastSeen">
        <h8>This recipee is in the last seen list !!</h8>
      </div>
      <div v-if="recipe.flagInFavorite">
        <h8>This recipee is in the favorite list !!</h8>
      </div> -->
    </div>
        <!-- <iconBar class="iconbar" :isFavorite="isFavorite" :isLastseen="isLastseen" :isVegetarian="isVegetarian" :isVegan="isVegan" :isGlutenfree="isGlutenfree" ></iconBar> -->
  </router-link>
</template>

<script>
// import iconBar from './iconBar.vue';
export default {
    mounted() {
        this.axios.get(this.recipe.image).then((i) => {
            this.image_load = true;
        });

        // this.isFavorite = this.$props.recipe.flagInFavorite;
        // this.isLastseen = this.$props.recipe.flagInLastSeen;
        // this.isVegan = this.$props.recipe.vegan;
        // this.isVegetarian = this.$props.recipe.vegetarian;
        // this.isGlutenfree = this.$props.recipe.glutenFree;


    },
    data() {
        return {
            image_load: false,
            // isFavorite:false,
            // isLastseen:false,
            // isVegan:false,
            // isVegetarian:false,
            // isGlutenfree:false
        };
    },
    props: {
        recipe: {
            type: Object,
            required: true
        }
        // id: {
        //   type: Number,
        //   required: true
        // },
        // title: {
        //   type: String,
        //   required: true
        // },
        // readyInMinutes: {
        //   type: Number,
        //   required: true
        // },
        // image: {
        //   type: String,
        //   required: true
        // },
        // aggregateLikes: {
        //   type: Number,
        //   required: false,
        //   default() {
        //     return undefined;
        //   }
        // }
    },
    // components: { iconBar }
};
</script>

<style scoped>
.recipe-preview {
  width: 90%;
  height: 100%;
  padding-bottom: 10px;
  margin-bottom: 1px;
  border-radius: 5px;

}
.recipe-preview > .recipe-body {
  height: 200px;
  margin-top: 22px;

} 

.recipe-preview .recipe-body .recipe-image {
  width: 98%;

}

.recipe-title {
  padding: 0px 5px;
  text-align: center;
  font-weight: bold;

}

ul.recipe-overview { 
  padding: 5px 10px;
  display: flex;
  margin-bottom: 0px;
}

ul.recipe-overview li {
  flex-grow: 1;
  display: table-cell;
  text-align: center;
  background-color: white;
  height: 30px;
  font-size: 15px;
  color: #0e0c0c;
  border-radius: 0.9vw;
  padding: 2px 0px;
  margin: 0px 3px;

}

#time-icon{
  width: 23px;
  height: 24px;
	margin-top: -5px;
                  
}

#glutenFree-icon{
    width: 45px;
  height: 45px;
	margin-top: -9px;
}

#vegan-icon {
    width: 45px;
  height: 45px;
	margin-top: -9px;
}
#vegetarian-icon{
    width: 45px;
  height: 45px;
	margin-top: -9px;
}

#like-icon{
      width: 22px;
  height: 22px;
	margin-top: -9px;
}

#favorite-icon{
   width: 23px;
  height: 14px;
	margin-top: -5px;
}
 
</style>
