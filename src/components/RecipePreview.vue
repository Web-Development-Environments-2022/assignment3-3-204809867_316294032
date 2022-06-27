<template>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
    class="recipe-preview"
  >
    <div class="recipe-body">
      <img v-if="image_load" :src="recipe.image" class="recipe-image" />
    </div>
    <div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title">
        {{ recipe.title }}
      </div>
      <ul class="recipe-overview">
        <li>{{ recipe.readyInMinutes }} minutes</li>
        <li>{{ recipe.popularity }} likes</li>
        <li>{{recipe.servings}} servings</li>
        <li>Is Vegan ?{{recipe.vegan}}</li>
        <li>Is Vegetarian ?{{recipe.Vegetarian}}</li>
        <li>Is glutenFree ?{{recipe.glutenFree}}</li>
        <li>Is In Favorite ?{{recipe.flagInFavorite}}</li>
        <li>Is In Last Seen ?{{recipe.flagInLastSeen}}</li>

        <!-- this in the main page! -->
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
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}
</style>
