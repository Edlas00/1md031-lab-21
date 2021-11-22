<template>
  <div style="display:inline-block;" >
    <h3>{{burger.name}}</h3>
    <img v-bind:src="burger.image" v-bind:title="burger.name" width="300" height="250">
    <ul>
      <li>{{burger.kCal}} kCal</li>
      <li>Contains <span class="ingredients"> gluten </span></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
