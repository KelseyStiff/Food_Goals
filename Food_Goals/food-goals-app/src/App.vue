<template>
  <div id="app">

    <!--    page header-->
    <div class="header">
      <h1 id="logo">Food Goals</h1>
      <div id="add-food-button">
        <img alt="add food" @click="showFoodSearch = true" src="@/assets/add-food-button.png">
        <br>
      </div>
    </div> <!--    end of page header-->



    <div class="macros-container">
      <div id="total-calories">
        <h1>{{ calorieTotal }} <br>
          <span>calories</span></h1>
      </div>
      <div class="macros-container-header">
        <h1>Stats</h1>
      </div>
      <div id="chart-style">
        <calories-chart v-bind:chartData="chartData"></calories-chart>
      </div>

    </div>

    <!--table container-->
    <div class="table-container" >
      <div class="table-container-header"><h1>Food Diary</h1></div>
      <food-table v-bind:savedFoods="savedFoods" v-on:delete-food="foodDeleted"></food-table>
    </div>


    <div id="notes-container">
      <div class="notes-header">
        <h1>Notes</h1>
      </div>
      <textarea id=notes-input type="text" placeholder="add notes..."></textarea>

    </div> <!--  end of notes container-->

    <!-- food search input container-->
    <div id="food-search">
      <food-search v-if="showFoodSearch" @close="showFoodSearch = false" v-on:food-added="newFoodAdded"></food-search>
    </div><!--    end of search input container-->

    <div id="creds">
      Icons made by: <a href="https://www.freepik.com" title="Freepik">Freepik</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a>
    </div>

  </div> <!--    end of app container-->
</template>

<script>


import FoodSearch from './components/FoodSearch.vue'
import FoodTable from "./components/FoodTable";
import CaloriesChart from "./components/CaloriesChart";

export default {
  name: 'App',
  components: {
    FoodSearch,
    FoodTable,
    CaloriesChart,
  },
  data(){
    return {
      savedFoods:  [],
      showFoodSearch: false,
    }
  },
  methods: {
    newFoodAdded(food){
      this.savedFoods.push(food)
    },
    foodDeleted(food){
      this.savedFoods = this.savedFoods.filter( function (f){
        if(f != food){
          return true
        }
      })
    }
  },
  computed: {
    chartData() {
      let labels
      let calories
        labels = this.savedFoods.map(rec => rec.name)
        calories = this.savedFoods.map(rec => rec.calories)

      return {
        labels: labels,
        datasets: [ {
          label: '',
          data: calories,
          backgroundColor: ['#fca8a8','#ffd48a','#7aabc4','#84dd9a', '#8860D0','#8860D0','#C4FFDB','#E483F8','#6AFFC1']
        }],
      }
    }
    ,
    calorieTotal: function (){
      if(this.savedFoods){
        //learned how to use .reduce method on stack overflow (by: OwChallie)
        return this.savedFoods.reduce(function (prev, cur) {
          return prev + cur.calories;
        }, 0)
      }
    }
  }
}
</script>



<style>

@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300&display=swap');
#app{
  font-family: 'Lato', sans-serif;;
}

body {
  background: #f3d0a3;
  height: auto;
  position: relative;
}

#add-food-button img{
  height: 50px;
  float: right;
  margin-right: 50px;
  padding-bottom: 3px;
}

#logo{
  float: left;
  color: #181818;
  font-size: 40px;
  margin: auto;
  padding-left: 15px;
}

.header{
  background: #fffafa;
  height: 50px;
  margin-bottom: 20px;
  width: 100%;
  -webkit-box-shadow: 0 5px 6px -6px #777;
  -moz-box-shadow: 0 5px 6px -6px #777;
  box-shadow: 0 5px 6px -6px #777;
  border-radius: 3px;
}

.macros-container {
  position: relative;
  background: white;
  width: 100%;
  height: 225px;
  margin: 10px 10px 10px 0px;
  -webkit-box-shadow: 0 8px 6px -6px #777;
  -moz-box-shadow: 0 8px 6px -6px #777;
  box-shadow: 0 8px 6px -6px #777;
}

.macros-container #total-calories h1{
  margin: auto;
  position: absolute;
  top: 40%;
  right: 15%;
  font-size: 50px;
}

.macros-container #total-calories h1 span{
  font-size: 15px;
  position: absolute;
  left: 20%;
}

.macros-container-header{
  background: #fffafa;
  width: 100%;
  height: 30px;
  display: block;
  text-align: center;
  border-bottom: #b7b5b5 solid 2px;
  position: absolute;
  top: 0;
}

.macros-container-header h1 {
  font-size: 20px;
  margin: auto;
}

.macros-container #chart-style{
  padding: 20px;
  width: 100%;
  max-width: 180px;
  height: 180px;
  position: absolute;
  top: 8%;
}

.table-container {
  float: left;
  background: white;
  min-width: 300px;
  max-width: 400px;
  height: 500px;
  -webkit-box-shadow: 0 8px 6px -6px #777;
  -moz-box-shadow: 0 8px 6px -6px #777;
  box-shadow: 0 8px 6px -6px #777;
  border-radius: 2px;
  position: relative;
  display: block;
  margin: 10px;
}

.table-container-header{
  background: #fffafa;
  width: 100%;
  height: 30px;
  display: block;
  text-align: center;
  border-bottom: #b7b5b5 solid 2px;
  position: absolute;
  top: 0;
}

.table-container-header h1{
  font-size: 20px;
  margin: auto;
}


#notes-container{
  float: right;
  background: white;
  width: 300px;
  height: 250px;
  -webkit-box-shadow: 0 8px 6px -6px #777;
  -moz-box-shadow: 0 8px 6px -6px #777;
  box-shadow: 0 8px 6px -6px #777;
  position: relative;
  margin: 10px 10px 0px 0px;
}

.notes-header{
  background: #fffafa;
  width: 100%;
  height: 30px;
  display: block;
  text-align: center;
  border-bottom: #b7b5b5 solid 2px;
  position: absolute;
  top: 0;
}

.notes-header h1 {
  font-size: 20px;
  margin: auto;
}

#notes-input{
  width: 90%;
  height: 70%;
  position: absolute;
  top: 21%;
  border-style: none;
}

::-webkit-input-placeholder {
  position: absolute;
  text-align: left;
}

#creds {
  color: lightgray;
  font-size: x-small;
  margin: auto;
  text-align: center;
  bottom: 0;
  position: fixed;
}

</style>
