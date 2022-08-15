<template>
  <div class="container">
    <h1 class="title">Main Page</h1>

    <div id="left">
      <RecipePreviewList title="Explore this recipes:" call_type="random" class="RandomRecipes center" />
    </div>
    <router-link id="ref-Login" v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue your history
    </router-link>
    <div id="right">
    <RecipePreviewList title="Last watched recipes:" call_type="lastWatch" :class="{
      RandomRecipes: true,
      blur: !$root.store.username,
      center: true
    }" disabled>
    </RecipePreviewList>
    </div>

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
  -webkit-filter: blur(5px);
  filter: blur(2px);
}

::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

#left{
  float: left;
  width: 350px;
}

#right{
  float: right;
  width: 350px;
}

#ref-Login{
  font-family: cursive;
    background: #f3c86d;
    box-shadow: 0 0 20px 9px #f147041f;
    color: rgb(7, 7, 6);
    padding: 5px;
    margin: 10px;
    transition: .5s;
    font-size: 18px;
    border: 0;
    border-radius: 10px;
    padding-top: 7px;
    padding-bottom: 7px;
}

</style>
