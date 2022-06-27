<template>
  <!-- <div class="container">
    <h1 class="title">Main Page</h1>
    <div class="split left">
      <div class="centered">
        <RecipePreviewList title="Explore this recipes" call_type="random" class="RandomRecipes center" />
      </div>
    </div>
    

    <div class="split right">
      <div class="centered">
        <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
        <RecipePreviewList title="Last watched recipes" call_type="lastWatch"
          :class="{
            RandomRecipes: true,
            blur: !$root.store.username,
            center: true}"
          disabled>
        </RecipePreviewList>
      </div>
    </div> -->

      <div class="container">
    <h1 class="title">Main Page</h1>

        
    <RecipePreviewList title="Explore this recipes" call_type="random" class="RandomRecipes center" />

   
        <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
        <RecipePreviewList title="Last watched recipes" call_type="lastWatch"
          :class="{
            RandomRecipes: true,
            blur: !$root.store.username,
            center: true}"
          disabled>
        </RecipePreviewList>




    

    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  components: {
    RecipePreviewList
  }
};
</script>











<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}



// /* Split the screen in half */
// .split {
//   height: 100%;
//   width: 50%;
//   position:absolute;
//   z-index: 1;
//   top: 0;
//   overflow-x: hidden;
//   padding-top: 20px;
// }

// /* Control the left side */
// .left {
//   left: 0;
// }

// /* Control the right side */
// .right {
//   right: 0;
// }

// /* If you want the content centered horizontally and vertically */
// .centered {
//   position: absolute;
//   top: 50%;
//   left: 50%;
//   transform: translate(-50%, -50%);
//   text-align: center;
// }

</style>
