<template>
  <b-container>
    <b-row>
      <b-col>
        <b-col v-if="dataflagiflagInFavorite">
          <img id="logoFavorite" src="../assets/logo/alreadyFavorite.png" />
        </b-col>
        <b-col v-else>
          <!-- <button v-on:click="updateFavorite()">Dire salut</button> -->
          <img id="logoNotFavorite" src="../assets/logo/clickForFavorite.png" @mouseover="hover = true"
            @mouseleave="hover = false" v-on:click="updateFavorite()" />
          <br>
          <span v-if="hover">Click to add as Favorite! </span>
        </b-col>

      </b-col>
      <b-col v-if="dataflagiflagInLastSeen">
        <h2>lastseen</h2>
      </b-col>
      <b-col v-if="dataflagiflagVegan">
        <h3>vegan
          <img id="vegan-icon" src="../assets/logo/vegan.png">
        </h3>
      </b-col>
      <b-col v-if="dataflagiflagVegetarian">
        <h3>vegetarian
          <img id="vegetarian-icon" src="../assets/logo/vegetarian.png">
        </h3>
      </b-col>
      <b-col v-if="dataflagiflagGlutenFree">
        <h3>glutenfree
          <img id="glutenFree-icon" src="../assets/logo/gluten-free.png">
        </h3>
      </b-col>
      <!-- <b-col v-if="flagiflagInFavorite">
        <h2>favorite</h2>
      </b-col>
      <b-col v-else>
        <h2>not favorite</h2>
      </b-col>
      <b-col v-if="flagiflagInLastSeen">
        <h2>lastseen</h2>
      </b-col>
      <b-col v-else>
        <h2>not lastseen</h2>
      </b-col>
      <b-col v-if="flagiflagVegan">
        <h2>vegan</h2>
      </b-col>
      <b-col v-else>
        <h2>not vegan</h2>
      </b-col>
      <b-col v-if="flagiflagVegetarian">
        <h2>vegetarian</h2>
      </b-col>
      <b-col v-else>
        <h2>not vegetarian</h2>
      </b-col>
      <b-col v-if="flagiflagGlutenFree">
        <h2>glutenfree</h2>
      </b-col>
      <b-col v-else>
        <h2>not glutenfree</h2>
      </b-col> -->
    </b-row>
  </b-container>
</template>

<script>
export default {
  components: {},
  props: {
    isFavorite: {
      type: Boolean,

    },
    isLastseen: {
      type: Boolean,

    },
    isVegan: {
      type: Boolean,

    },
    isVegetarian: {
      type: Boolean,

    },
    isGlutenfree: {
      type: Boolean,

    }



  },
  data() {

    return {
      hover: false,
      //   logoFavorite: require("../assets/logo/alreadyFavorite.png"),
      //   logoNotFavorite: require("../assets/logo/clickForFavorite.png"),
      dataflagiflagInFavorite: false,
      dataflagiflagInLastSeen: false,
      dataflagiflagVegan: false,
      dataflagiflagVegetarian: false,
      dataflagiflagGlutenFree: false
    };
  },
  created() {
    console.log(this.$props.isFavorite)
    console.log(this.$props.isLastseen)
    console.log(this.$props.isVegan)
    console.log(this.$props.isVegetarian)
    console.log(this.$props.isGlutenfree)

    if ((this.$props.isFavorite == "true") || (this.$props.isFavorite == true)) {
      this.dataflagiflagInFavorite = true;
    }
    if (this.$props.isLastseen == "true" || (this.$props.isLastseen == true)) {
      this.dataflagiflagInLastSeen = true;
    }
    if (this.$props.isVegan == "true" || (this.$props.isVegan == true)) {
      this.dataflagiflagVegan = true;
    }
    if (this.$props.isVegetarian == "true" || (this.$props.isVegetarian == true)) {
      this.dataflagiflagVegetarian = true;
    }
    if (this.$props.isGlutenfree == "true" || (this.$props.isGlutenfree == true)) {
      this.dataflagiflagGlutenFree = true;
    }

    // this.dataflagiflagInLastSeen = this.$props.isLastseen;
    // this.dataflagiflagVegan = this.$props.isVegan;
    // this.dataflagiflagVegetarian = this.$props.isVegetarian;
    // this.dataflagiflagGlutenFree = this.$props.isGlutenfree;


  },
  methods: {
    async updateFavorite() {
      this.dataflagiflagInFavorite = true
      try {
        const response = await this.axios.post(this.$root.store.server_domain + "/users/favorites", {
          recipeId: this.$route.params.recipeId
        }, { withCredentials: true });

      }
      catch (err) {
        console.log("error.response.status", error.response.status);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  height: 55px;
}

#logoFavorite {
  width: 70px;
  height: 50px;
}

#logoNotFavorite {
  width: 70px;
  height: 50px;
}

#glutenFree-icon {
  width: 45px;
  height: 45px;
  margin-top: -10px;
}

#vegan-icon {
  width: 45px;
  height: 45px;
  margin-top: -10px;
}

#vegetarian-icon {
  width: 45px;
  height: 45px;
  margin-top: -10px;
}
</style>
