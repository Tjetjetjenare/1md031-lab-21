<template>
<header>
  <img src="https://wallpaperfx.com/view_image/vintage-pub-1440x900-wallpaper-13796.jpg" alt="headpicture">
  <h1 class="title">Welcome to Glorious Borger</h1>
</header>
  <main>
    <div class="menu">
      <h2>Please select your burger</h2>
      <p>This is where you select the burger you want to order</p>
      <div class="burgers">
        <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
      </div>
    </div>
      <!--<section id="picblock" class="menu burgers">
        <h2>Please select your burger</h2>
        <p class="burger1">This is where you select the burger you want to order</p>
          <div class="burger1">
            <h4>Dantes Inferno</h4>
            <img src="https://cdn11.bigcommerce.com/s-cg0adf/product_images/uploaded_images/cotw-brand-heatguide-4.jpg" alt="spicyness1" style="width: 100px">
            <img src="https://thumbs.dreamstime.com/b/spicy-burger-marinated-onions-chilli-wooden-board-dark-background-spicy-burger-marinated-onions-chilli-225658874.jpg" alt="spicy burger" style="width: 300px">
            <ul>
              <li>1 200kcal</li>
              <li>Contains <span id="boldie">lactose</span></li>
              <li>Contains <span id="boldie">gluten</span> </li>
            </ul>
          </div>
          <div class="burger2">
            <h4>Crispy Chicken Star Struck</h4>
            <img src="https://cdn11.bigcommerce.com/s-cg0adf/product_images/uploaded_images/cotw-brand-heatguide-1.jpg" alt="spicyness2" style="width: 100px">
            <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ3H6EHrJuBqHMBCcVRb-3YiGbq0BWgAx5JQA&usqp=CAU" alt="chicken burger" style="height: 200px">
            <ul>
              <li>800kcal</li>
              <li> Contains <span id="boldie">lactose</span> </li>
            </ul>
          </div>
          <div class="burger3">
            <h4>Halloumi Maximus</h4>
            <img src="https://cdn11.bigcommerce.com/s-cg0adf/product_images/uploaded_images/cotw-brand-heatguide-2.jpg" alt="spicyness3" style="width: 100px">
            <img src="https://www.kitchensanctuary.com/wp-content/uploads/2015/11/Halloumi-Burger-with-Sticky-Chilli-Glaze-square-FS-50.jpg" alt="halloumi burger" style="height: 200px">
            <ul>
              <li>1 000kcal</li>
              <li class="dogreen">Vegan</li>
            </ul>
          </div>-->

    <section class="cumstomersection">
      <h2>Customer information</h2>
        <p>Please enter the following information:</p>
      <p>
        <label for="Full name">Full name</label><br>
        <input type="text" id="full name" v-model="fn" required="required" placeholder="First- and last name">
      </p>
      <p>
        <label for="E-mail">E-mail</label><br>
        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
      </p>
      <!--<p>
        <label for="Street name">Street</label><br>
        <input type="text" id="Street name" v-model="sn" required="required" placeholder="Street name">
      </p>-->
      <!--<p>
        <label for="House number">House</label><br>
        <input type="number" id="House number" v-model="hn" required="required" placeholder="House number">
      </p>-->
      <p>
        <label for="recipient">Choose method of payment</label><br>
        <select id="recipient" v-model="rcp">
          <option disabled value="">Please select</option>
          <option>Crypto currency</option>
          <option>Credit card</option>
          <option>Swish</option>
          <option>Klarna</option>
          <option>Cash* **</option>
        </select><br>
        *Cash can be payed to the delivery person <br>
        **For their safety, they will not carry change
      </p>
      <p>
        Gender<br>
        <input type="radio" id="male" name="gender" value="male" v-model="picked">
        <label for="Gender">Male</label><br>
        <input type="radio" id="female" name="gender" value="female" v-model="picked">
        <label for="Gender">Female</label><br>
        <input type="radio" id="non-binary" name="gender" value="non-binary" v-model="picked">
        <label for="Gender">Non-binary</label><br>
        <input type="radio" id="undisclosed" name="gender" value="Undisclosed" v-model="picked" checked="checked">
        <label for="Gender">Undisclosed</label>
      </p>
      <h4>Choose where you want your food to be delivered</h4>
      <div class="karta">
      <div id="map" v-on:click="setLocation">{{location}}
        <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
          T
        </div>
      </div>
      </div>
    </section>
    <section>
      <button v-on:click="placeOrder" type="submit">
        <img src="https://c8.alamy.com/comp/M8741Y/a-vector-cartoon-representing-a-funny-and-fast-green-fresh-food-delivery-M8741Y.jpg" style="width:25px">
        Bring me my food
      </button>
    </section>
  </main>
  <hr>
  <footer>
    &copy; Tjetjetjenare inc.
  </footer>
</template>

<script>
import menu from "../assets/menu.json"
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

console.log(menu);

const socket = io();

/*function MenuItem(name, url1, url2, kcal, l, g, o){
  this.name = name;
  this.url1 = url1;
  this.url2 = url2;
  this.kcal = kcal;
  this.lactose = l;
  this.gluten = g;
  this.other = o;
}*/

/*const minaBurgare = [new MenuItem("Dante's Inferno", "https://cdn11.bigcommerce.com/s-cg0adf/product_images/uploaded_images/cotw-brand-heatguide-4.jpg", "https://thumbs.dreamstime.com/b/spicy-burger-marinated-onions-chilli-wooden-board-dark-background-spicy-burger-marinated-onions-chilli-225658874.jpg", "1 200kcal", true, true, false),
                 new MenuItem("Crispy Chicken Star Struck", "https://cdn11.bigcommerce.com/s-cg0adf/product_images/uploaded_images/cotw-brand-heatguide-1.jpg", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ3H6EHrJuBqHMBCcVRb-3YiGbq0BWgAx5JQA&usqp=CAU", "800kcal", true, false, false),
                 new MenuItem("Halloumi Maximus", "https://cdn11.bigcommerce.com/s-cg0adf/product_images/uploaded_images/cotw-brand-heatguide-2.jpg", "https://www.kitchensanctuary.com/wp-content/uploads/2015/11/Halloumi-Burger-with-Sticky-Chilli-Glaze-square-FS-50.jpg", "1 000kcal", false, false, "vegan")
                ];

console.log(minaBurgare);*/

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fn: "",
      em: "",
      //sn: "",
      //hn: "",
      rcp: "",
      picked: "",
      amountOrdered: "",
      orderedBurgers: {},
      personalInformation: {},
      location: {x: 0,
                 y: 0}
    }
  },
  methods: {
    placeOrder: function () {
      this.personalInformation = {
        name: this.fn,
        email: this.em,
        payment: this.rcp,
        gender: this.picked
      }
      console.log(
        this.fn,
        this.em,
        //this.sn,
        //this.hn,
        this.rcp,
        this.picked,
        this.amountOrdered,
        this.orderedBurgers
      );
      socket.emit("addOrder", {
                                orderId: this.getOrderNumber(),
                                details: this.location,
                                personalInformation: this.personalInformation,
                                orderItems: this.orderedBurgers
                              }
                 );
    },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addToOrder: function (event){
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y
      }
    }
    /*addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};*/
  },
}

</script>

<style>
  #map {
    cursor: crosshair;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    position: relative;
  }
  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width: 10px;
    height: 15px;
    text-align: center;
  }
.karta {
  overflow: scroll;
}
@import 'https://fonts.googleapis.com/css2?family=Shadows+Into+Light&display=swap';
/*Body*/
body {
  font-family: "Droid Serif", sans-serif;
}
/*Section*/
.cumstomersection {
  margin: 5px 0px;
  padding: 10px;
  padding-right: 200px;
  border-width: 2px;
  border-style: dashed;
  border-color: #000000
}
/*Div*/
div {
  margin: 5px 10px;
  padding: 5px 10px;
}
/*Header*/
header {
  height: 300px;
  overflow: hidden;
}
header > img {
  opacity: 0.7;
  width: 100%;
  height: 450px;
}
/*Text*/
.title {
  position: absolute;
  margin-top: -425px;
  padding: 60px 500px 50px 600px;
  font-family: 'shadows Into Light', cursive;
}
/*Grid*/
.menu {
  background-color: #000000;
  color: #ffffff;
  border: 2px dashed #ffffff;
  padding: 10px;
}

.menu div {
  padding: 10px;
}

.burgers {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 33% 33% 33%;
}

/*Färger*/
.dogreen {
  color: #00ff00;
}

.doblack {
  color: #000000;
}

.dowhite {
  color: #ffffff;
}

.doblackbackground {
  background-color: #000000;
}

#transparent {
  opacity: 0.5;
}
/*Texttyper*/
#boldie {
  font-weight: bold;
}
/*Blocking*/
#picblock img{
  display: block;
}
/*Button*/
button {
  margin: 20px 0px 0px 0px;
}
button:hover {
  background-color: #0090ff;
  cursor:pointer;
}

</style>
