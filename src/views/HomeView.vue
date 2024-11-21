<template>
  <div>
    <header>
            <img id="headerImg" src="/img/bar.jpeg">
            <h1 id="headerText">Välkommen till BurgerOnline</h1>
    </header>   
    <main>
        <section id="burger">
          <h2>Select burger</h2>
          <p>This is where you execute burger selection</p>
          <div class="wrapper">
            <Burger v-for="burger in burgers"
                    v-bind:burger="burger"
                    v-bind:key="burger.name"
                    v-on:orderedBurger="addToOrder($event)">
            </Burger>
          </div>
        </section>
        <section id="information">
            <h2>Costumer information</h2>
            <p>This is were you provide neccessary information</p>
            <h3>Customer information</h3>
            <form>
                <p>
                    <label for="fullname">Full name: {{ fn }}</label><br>
                    <input type="text" id="fullname" v-model="fn" required="required" placeholder="Full name">
                </p>
                <p>
                    <label for="email">E-mail adress: {{ em }}</label><br>
                    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail adress">
                </p>
                <!--<p>
                    <label for="street">Street name: {{ sn }}</label><br>
                    <input type="text" id="street" v-model="sn" required="required" placeholder="Street name">
                </p>
                <p>
                    <label for="house">House number {{ hn }}</label><br>
                    <input type="number" id="house" v-model="hn" required="required" placeholder="House number">
                </p>-->
                <p>
                    <label for="payment">Payment method: {{ pm }}</label><br>
                    <select id="payment" v-model="pm">
                        <option selected="selected">Card</option>
                        <option>Klarna</option>
                        <option>Swish</option>
                        <option>Gift card</option>
                    </select>                
                </p>
                <p>
                    <label for="radio">Gender: {{ gender }}</label> <br>
                    <input type="radio" id="female" v-model="gender" value="Female">
                    <label for="female">Female</label>
                    <br>
                    <input type="radio" id="male" v-model="gender" value="Male">
                    <label for="male">Male</label>
                    <br>
                    <input type="radio" id="none" v-model="gender" value="None" checked="checked">
                    <label for="none">Do not wish to provide</label>
                    <br>
                </p>
            </form>
          <div id="mapframe">
            <div id="map" v-on:click="setLocation($event)">
              <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px',position:'absolute', height:'10px', width:'10px' }">
                T
              </div>
            </div>
          </div>
        </section>
        <button type="submit" id="sendbutton" v-on:click="sendOrder()">
            <img src="/img/sendbutton.jpeg" style="width:20px">
            Place my order!
        </button>
    </main>
    <hr>
    <footer>
        <p>&copy; 2003 Teas goda hamburgare Inc.</p>
    </footer>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name,url,kCal,gluten,lactose){
    this.name = name;
    this.url =url;
    this.kCal = kCal;
    this.gluten=gluten;
    this.lactose=lactose;
};
/*
const coffeBurger = new MenuItem("Coffee Burger", '/img/coffeburger.png',1200,true,false);
const teaBurger = new MenuItem("Tea Burger", '/img/teaburger.jpg', 1000,true,true);
const candyBurger = new MenuItem("Candy Burger",'/img/candyburger.jpg',1900,false,false);
*/
//const burgers = [coffeBurger,teaBurger,candyBurger];
//console.log(burgers);
const coffeBurger=menu[0]
const teaBurger=menu[1]
const candyBurger=menu[2]
export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: [ coffeBurger,
                 teaBurger,
                 candyBurger
               ],
      gender:'Do not wish to provide',
      pm: 'Card',
      fn:'',
      em:'',
      //sn:'',
      //hn:'',
      orderedBurgers:{},
      location:{x:0,y:0}
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      
    },
    addToOrder: function (event){
      console.log("hej")
      this.orderedBurgers[event.name]=event.amount
    },
    setLocation: function(event){
      var offset = {x:event.currentTarget.getBoundingClientRect().left,
        y:event.currentTarget.getBoundingClientRect().top
      };
      this.location.x=event.clientX-offset.x
      this.location.y=event.clientY-offset.y
      console.log(this.location.x)
    },
    sendOrder: function(event){
      console.log(this.orderedBurgers)
      console.log(this.fn,this.em,this.pm,this.gender,this.orderedBurgers)
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           name: this.fn,
                                           mail: this.em,
                                           pay:this.pm,
                                           gender:this.gender },
                                orderItems: this.orderedBurgers
                              }
                 );
    }
  }
}
</script>

<style>
  #mapframe{
    width: 100%;
    height:250px; 
    overflow:scroll;
    position: relative;
  }
  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    position: relative;
  }
  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
  body {
    font-size: 12pt;
    font-family:'Times New Roman', Times, serif;
  }

  p {
    color: red;
  }

  h1 {
    font-family: 'Agbalumo';
    font-size: 32pt;
    text-align: center;
  }
  
  main {
    background-color: bisque;
  }

  /* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
  } */

  header {
    /*background-image: url("../img/polacks.jpg");
    background-size: cover;*/
    overflow: hidden;
    width: 100%;
    height: 200px;
    padding: 5px;
  }

  #headerImg{
    opacity: 0.5;
    height:auto;
    width:100%;
  }
  #headerText{
    margin-left:5px;
    position:absolute;
    margin-top:-450px;
  }

  header h1 {
    width:40rem;
    margin: 0 auto;
    text-align: center;
  }

  nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
  }

  nav li {
    display: block;
    background-color: grey;
    padding: 1em;
  }

  .Very-good {
    color: green;
  }

  .Master {
    color: green;
    font-weight: bold;
  }

  .bold{
    font-weight: bold;
  }

  #burger {
    background-color: black;
    color:white;
    border: 3px dashed;
    border-color:white;
  }

  #information{
    border: 3px dashed;
    border-color:black;
  }

  button:hover{
    background-color: rgb(67, 66, 66);
    cursor:pointer;
  }

  section h2{
    margin-top: 20px;
    margin-left:10px;
  }
  section form,h2,p,h3{
    margin-left: 10px;
  }

  #sendbutton{
    margin-top:15px;
  }
  .wrapper{
    display: grid;
    grid-gap: 100px;
    grid-template-columns: 100px 100px 100px;
  }

    
</style>