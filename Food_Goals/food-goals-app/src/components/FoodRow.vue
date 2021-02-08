<template>
  <tr>
    <td>{{ savedFood.name }}</td>
    <td>{{ savedFood.calories }}</td>
    <td v-show="edit"><img v-on:click="deleteFood" src="@/assets/delete.png"></td>
  </tr>
</template>

<script>
export default {
  name: "FoodRow",
  props: {
    savedFood: Object,
    edit: Boolean
  },
  methods: {
    deleteFood(){
      this.$emit('delete-food', this.savedFood)
    }
  }
}
</script>

<style scoped>

img {
  height: 25px;
}

td {
  padding: 5px;
}

tr {
  overflow-y:hidden;
  margin: auto;
}
</style>