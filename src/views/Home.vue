<template>
  <div>
    Burgers
    <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"/>
  </div>
  <div id="map" v-on:click="addOrder">
    click here
  </div>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

const Burger_items = [
  new MenuItem("The Classic","https://images.kitchenstories.io/recipeImages/RP16_01_171_ClassicHamburger_Final_4x3.jpg",
  600, true, false), new MenuItem("Bacon Burger","https://recipe-graphics.grocerywebsite.com/0_GraphicsRecipes/4589_4k.jpg",
  800, false, true), new MenuItem("Spicy burger","https://imagesvc.meredithcorp.io/v3/mm/image?url=https%3A%2F%2Fstatic.onecms.io%2Fwp-content%2Fuploads%2Fsites%2F52%2F2019%2F05%2F01%2Fspicy-beef-pork-burgers-with-bacon-barbecued-onions-102383216.jpg&amp;q=85"
  , 850, false, false)
]

console.log(Burger_items);

function MenuItem(name, URL, kCal, gluten, lactose){
  this.name = name;
  this.image = URL;
  this.kCal = kCal;
  this.gluten = Boolean(gluten);
  this.lactose = Boolean(lactose);
}

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: Burger_items
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
