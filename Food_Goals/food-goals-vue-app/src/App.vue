<template>
  <div id="app">
    <input
        type="text"
        v-model.trim="search"
        placeholder="Search foods..."
        @keyup="searchAllFoods"
    />
    <br>
    <ul>
      <li v-for="food in foods" :key="food.id" v-on:click="viewFoodDetails">
        {{ food.description }}
      </li>
    </ul>


  </div>
</template>

<script>
let axios = require('axios');
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data(){
    return {
      foods: {},
      search: '',
      URL: 'https://api.nal.usda.gov/fdc/v1/foods/search?',
      APIKEY: 'api_key=4lJM36HZ5LpjaQ7bei6cC4JlX6C5xBqohu96XFXH',
      searchResultUrl: '',
      foodName: '',
      foodServing: '',
      foodCalories: '',
      foodMacros: []


    }
  },
  methods: {
    searchAllFoods() {
      let query = this.search
      this.searchResultUrl = this.URL+ this.APIKEY + '&query=' + query
      if (this.search) {
        fetch(this.searchResultUrl)
            .then(response => response.json())
            .then(res => {
              this.foods = res.foods
            });
      } else if(!this.search) {
        this.foods = []
      }
      return this.searchResultUrl, this.foods
    },
  },
  created() {
    this.searchAllFoods();
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
