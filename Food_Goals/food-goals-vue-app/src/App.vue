<template>
  <div id="app">

    <button @click="showFoodSearch = true">ADD FOOD</button>

    <food-search v-bind:allFoods="allFoods"
                v-if="showFoodSearch"
                @close="showFoodSearch = false"
                 v-on:food-added="newFoodAdded"
                 v-on:delete-food="foodDeleted"
                 >
    </food-search>

    <food-table v-bind:savedFoods="savedFoods" v-on:delete-food="foodDeleted">

    </food-table>


  </div>
</template>

<script>
import foodSearch from './components/foodSearch.vue'
import foodTable from './components/foodTable.vue'
export default {
  name: 'App',
  components: {
    foodSearch,
    foodTable
  },
  data(){
    return {
      allFoods: {},
      savedFoods: [],
      showFoodSearch: false,
    }
  },
  methods: {
    newFoodAdded(newFood){
      this.savedFoods.push(newFood)
    },
    foodDeleted(newFood){
      this.$delete(this.savedFoods, newFood)
    }
  }

}



</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
