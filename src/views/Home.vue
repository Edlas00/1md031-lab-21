<template>

<header class="head">
      <img src="https://c0.wallpaperflare.com/preview/565/228/846/japanese-japan-restaurant-culture.jpg" alt="">
      <h1>Välkommen till BurgerOnline</h1>
  </header>
  <main>
    <section id="burger_selection">
    <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"
            />
      </section>

      <section id="order_info">
        <h2>Customer information</h2>
        <p>This is where you provide necessary information</p>
        <h3>Delivery information:</h3>
        <form id="contact">
          <p>
            <label for="fullname">Full name</label><br>
            <input type="text" id="fullname" v-model="name" required="required" placeholder="First- and Last name">
          </p>
          <p>
            <label for="email">E-mail</label><br>
            <input type="email" v-model="mail" id="email" required="required" placeholder="E-mail address">
          </p>
          <p>
            <label for="payment">Payment options</label><br>
            <select id="payment" v-model="selected_pay">
              <option>Credit card</option>
              <option>Swish</option>
              <option>Cash</option>
              <option>Gift code</option>
            </select>
          </p>
          <p>Gender:</p>
          <div id="v-model-radiobutton">
            <input type="radio" id="male" v-model="gender" value="male">
            <label for="male">Male</label>
          </div>
          <div>
            <input type="radio" id="female" v-model="gender" value="female">
            <label for="female">Female</label>
          </div>
          <div>
            <input type="radio" id="undisclosed" v-model="gender" value="undisclosed">
            <label for="undisclosed">Undisclosed</label>
          </div>
        </form>
      </section>
      <label>Ange din plats</label>
      <div id="wrap">
      <div id="map" v-on:click="changeLocation"></div>
      <div id="location" v-bind:style="{ left:location.x + 'px', top:location.y +'px'}">
      T
      </div>
      </div>
      <button type="submit" v-on:click="submit">
        <img src="https://w1.pngwing.com/pngs/546/859/png-transparent-food-icon-delivery-icon-sushi-pizza-delivery-scooter-courier-food-delivery-text-thumbnail.png" style="width:20px;">
        Place my order!
      </button>

    </main>

    <footer>
      <hr>
      <p>© 2021 Hypothetical LaserBurgers Inc.</p>
    </footer>

</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

const Burger_items = [
  new MenuItem("The Classic","https://images.kitchenstories.io/recipeImages/RP16_01_171_ClassicHamburger_Final_4x3.jpg",
  600,true, false), new MenuItem("Bacon Burger","https://recipe-graphics.grocerywebsite.com/0_GraphicsRecipes/4589_4k.jpg",
  800, false, true), new MenuItem("Spicy burger","https://imagesvc.meredithcorp.io/v3/mm/image?url=https%3A%2F%2Fstatic.onecms.io%2Fwp-content%2Fuploads%2Fsites%2F52%2F2019%2F05%2F01%2Fspicy-beef-pork-burgers-with-bacon-barbecued-onions-102383216.jpg&amp;q=85"
  , 850, false, false)
];

console.log(Burger_items);

function MenuItem(name, image, kCal, gluten, lactose){
  this.name = name;
  this.image = image;
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
      burgers: menu,
      name: "",
      mail: "",
      selected_pay: "",
      gender: "",
      orderedBurgers:{Classic:0, BaconBurger:0, SpicyBurger:0},
      location:{x:0,y:0}
    }
  }
  ,
  methods: {
    changeLocation: function(event){
      const offset=event.target.getBoundingClientRect();
      this.location.x=event.clientX-offset.left-10;
      this.location.y=event.clientY-offset.top-10;
    },

    addToOrder: function(event){
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers);
    },

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
    },
    submit: function(){
      this.name;
      this.mail;
      this.selected_pay;
      this.gender;
      console.log(this.orderedBurgers);
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y},
                                orderItems: this.orderedBurgers,
                                persInfo:{name:this.name, email:this.mail,
                                pay:this.selected_pay, gender:this.gender}
                              });


    }


  }
}
</script>

<style>
  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
  }

  #wrap {
    overflow:scroll;
    width: 700px;
    height: 500px;
    position:relative;
  }

  #location{
    position:absolute;
    background-color:black;
    border-radius:15px;
    width:20px;
    height:20px;
    text-align:center;
    color:white;
  }

  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

  body {
    font-family: arial;
  }

  .ingredients{
    font-weight: bold;
  }

  #burger_selection{
    color: white;
    background-color: black;
    border: 2px dashed white;
  }

  #burger_selection > div{
    margin: 10px;
    padding: 10px;
  }

  #order_info{
    border: 2px dashed black;
  }

  button:hover{
    background-color: green;
    cursor: pointer;
  }

  section{
    margin: 5px 10px 5px 10px;
    padding-left: 10px;
    border-radius: 10px;
  }

  button{
    margin-top: 10px;
    margin-left: 10px;
  }

  .head{
    margin-left: 10px;
    margin-right: 10px;
    height: 200px;
    overflow: hidden;
  }

  .head > img{
    opacity: 0.75;
    width: 100%;
    height: auto;
  }

  .head > h1{
    position: absolute;
    opacity: 1;
    top: 10%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .wrapper{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 350px 350px 350px;
  }



</style>
