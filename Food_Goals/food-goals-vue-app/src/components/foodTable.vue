<!--table component to hold transaction row components-->
<template>
  <div>

      <table class="table">
        <tr>
          <th>Name</th>
          <th>Calories</th>

          <!--         if delete check box selected, delete table row will appear-->
          <p v-show="editTable"></p>
        </tr>

        <!--        when delete transaction event is emitted, transaction row is removed from table-->
        <food-row
            v-for="newFood in savedFoods"
            v-bind:newFood="newFood"
            v-bind:edit="editTable"
            v-bind:key="newFood.key"
            v-on:delete-food="deleteFood">
        </food-row>

      </table>


    <div class="edit-table">
      <input id="edit-table" type="checkbox" class="form-check-input" v-model="editTable">
      <label for="edit-table" class="form-check-label">delete Food?</label>
    </div>

  </div>
</template>

<script>

import foodRow from '@/components/foodRow.vue'

export default {
  name: 'TransactionTable',
  components: { foodRow },
  props: {
    savedFoods: Array
  },
  data(){
    return {
      editTable: false
    }
  },
  methods: {
    deleteFood(newFood){
      this.$emit('delete-food', newFood)
    }
  }
}
</script>

<style>

table, th {
  border: 2px solid #d2b0e5;
  border-collapse: collapse;
  padding: 5px;
  width: auto;

}

.edit-table{
  display: block;
  text-align: center;
  font-size: small;
}

</style>