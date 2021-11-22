<template>
  <div style="display:inline-block;" >
    <h3>{{burger.name}}</h3>
    <img v-bind:src="burger.image" v-bind:title="burger.name" width="300" height="250">
    <ul>
      <li>{{burger.kCal}} kCal</li>
      <li>Contains<span class="ingredients"> gluten:</span> {{burger.gluten}}</li>
      <li>Contains<span class="ingredients"> lactose:</span> {{burger.lactose}}</li>
    </ul>
    <section>
    Ordered: {{amountOrdered}}
    </section>

    <button v-on:click="addBurger" class="addBurger"> +1 </button>
    <button v-on:click="minBurger" class="minBurger"> -1 </button>


  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },

  data: function () {
    return {
      amountOrdered: 0,
    }
  },

  methods:{
    addBurger: function(){
      this.amountOrdered += 1;
      this.$emit('orderedBurger', { name:   this.burger.name,
                                amount: this.amountOrdered
                             }  )
    },

    minBurger: function(){
      this.amountOrdered -= 1;
      if(this.amountOrdered<0){
        this.amountOrdered=0;
      }
      this.$emit('orderedBurger', { name:   this.burger.name,
                                amount: this.amountOrdered
                              });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.addBurger{
  background-color: green;
}

.minBurger{
  background-color:red;
}

</style>
