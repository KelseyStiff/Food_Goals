<template>
  <div>

    <transition name="modal">
      <div class="modal-mask">
        <div class="modal-wrapper">
          <div class="modal-container">

            <div class="modal-body">
              <slot name="body">
                <!-- use the modal component, pass in the prop -->

                <foodDetails v-if="showModal"
                             @close="showModal = false"
                             v-bind:selectedFood="selectedFood"
                             v-on:add-food="addNewFood"
                ></foodDetails>

                <input
                    type="text"
                    v-model.trim="search"
                    placeholder="Search foods..."
                    @keyup="searchAllFoods"
                />

                <div id="food-search-results-list">
                  <ul class="list">
                    <li v-for="food in allFoods"
                        v-bind:key="food.key"
                        @click="showModal = true; showFoodDetails(food)"
                        >
                      {{ food.description }}
                    </li>
                  </ul>
                </div>

              </slot>
            </div>

            <button class="modal-default-button" @click="$emit('close')">
              CLOSE
            </button>

          </div>
        </div>
      </div>
    </transition>

  </div>




</template>

<script>
import foodDetails from '@/components/foodDetails.vue'

export default {
  name: 'food-search',
  components: {
    foodDetails
  },
  data: function () {
    return {
      showModal: false,
      allFoods: [],
      selectedFood: {},
      search: '',
      URL: 'https://api.nal.usda.gov/fdc/v1/foods/search?',
      APIKEY: 'api_key=4lJM36HZ5LpjaQ7bei6cC4JlX6C5xBqohu96XFXH',
      showFoodSearch: false,
      searchResultUrl: '',
    };
  },
  methods: {
    searchAllFoods() {
      let query = this.search
      this.searchResultUrl = this.URL+ this.APIKEY + '&query=' + query
      if (this.search) {
        fetch(this.searchResultUrl)
            .then(response => response.json())
            .then(res => {
              this.allFoods = res.foods
            });
      } else if(!this.search) {
        this.allFoods = {}
      }
      return this.searchResultUrl, this.allFoods
    },
    showFoodDetails(food){
      this.selectedFood = food
    },
    addNewFood(newFood){
      this.$emit('food-added', newFood)
    },
    deleteFood(newFood){
      this.$emit('delete-food', newFood)
    }
  },
  created() {
    this.searchAllFoods();
  }
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
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
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

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
</style>
