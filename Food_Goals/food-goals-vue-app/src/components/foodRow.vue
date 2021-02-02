<template>
<tr>
  <td>{{ newFood.name }}</td>
  <td>{{ newFood.calories }}</td>
  <td v-show="edit"><img v-on:click="deleteFood" src="@/assets/delete.png"></td>
</tr>
</template>

<script>
export default {
name: "foodRow",
  props: {
    newFood: Object,
    edit: Boolean
  },
  methods: {
    deleteFood(){
      this.$emit('delete-food', this.newFood)
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
</style>