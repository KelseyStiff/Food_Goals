<template>

  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <p>{{ selectedFood.description }}</p>
          <p>CALORIES: {{ selectedFoodCalories }}</p>

          <button v-on:click="addFood()" @click="$emit('close')">ADD FOOD</button>
          <div class="close-button topright">
            <button @click="$emit('close')">X</button>
          </div>

        </div>
      </div>
    </div>
  </transition>

</template>

<script>
export default {
  name: "foodDetails",
  props: {
    selectedFood: Object,
    selectedFoodCalories: Number,
    showFoodSearch: Boolean,
    search: String
  },
  data: function() {
    return {
      foodCalories: 0
    }
  },
  methods: {
    addFood(){
      let calories
      let carbs
      let protein
      let fat
      this.selectedFood.foodNutrients.forEach(function (nutrient){
        if(nutrient.nutrientName == 'Energy') {
          calories = nutrient.value
        }
        if(nutrient.nutrientName == 'Protein'){
          protein = nutrient.value
        }
        if(nutrient.nutrientName == 'Carbohydrate, by difference'){
          carbs = nutrient.value
        }
        if(nutrient.nutrientName == 'Total lipid (fat)'){
          fat = nutrient.value
        }
      })
      let food = {
        name: this.selectedFood.description,
        calories: calories,
        protein: protein,
        carbs: carbs,
        fat: fat,
      }
      this.$emit('add-food', food)
    },
  },

}

</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
  position: relative;
}


.modal-body {
  margin: 20px 0;
}

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

.close-button button {
  border: none;
  display: inline-block;
  vertical-align: middle;
  overflow: hidden;
  text-decoration: none;
  text-align: center;
  white-space: nowrap;
  background-color: Transparent;
  background-repeat:no-repeat;
  cursor:pointer;
  outline:none;
}

.topright{
  position: absolute;
  right: 0;
  top: 0;
}
</style>