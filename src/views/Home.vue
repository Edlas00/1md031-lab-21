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
            />
      </section>

      <section id="order_info">
        <h2>Customer information</h2>
        <p>This is where you provide necessary information</p>
        <h3>Delivery information:</h3>
        <form id="contact">
          <p>
            <label for="fullname">Full name</label><br>
            <input type="text" id="fullname" name="fn" required="required" placeholder="First- and Last name">
          </p>
          <p>
            <label for="email">E-mail</label><br>
            <input type="email" name="em" required="required" placeholder="E-mail address">
          </p>
          <p>
            <label for="street">Street</label><br>
            <input type="text" name="str" required="required" placeholder="Street name">
          </p>
          <p>
            <label for="house">House</label><br>
            <input type="number" name="nmb" required="required" placeholder="House number">
          </p>
          <p>
            <label for="payment">Payment options</label><br>
            <select id="payment" name="pay">
              <option>Credit card</option>
              <option>Swish</option>
              <option>Cash</option>
              <option>Gift code</option>
            </select>
          </p>
          <p>Gender:</p>
          <div>
            <input type="radio" id="male" name="gender" value="male">
            <label for="male">Male</label>
          </div>
          <div>
            <input type="radio" id="female" name="gender" value="female">
            <label for="female">Female</label>
          </div>
          <div>
            <input type="radio" id="undisclosed" name="gender" value="undisclosed">
            <label for="undisclosed">Undisclosed</label>
          </div>
        </form>
      </section>
      <button type="submit" name="button">
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
      burgers: menu
    }
  }
  ,
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
  /*#map {
    width: 300px;
    height: 300px;
    background-color: red;
  }*/

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
