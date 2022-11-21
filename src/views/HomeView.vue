

<template>
  <div>
    <header class="front">

      <img id="bild1" src = "https://cdn.wallpapersafari.com/2/79/mXQSHz.jpg"
        alt = Restarang title = Restaurang >
    <h1 id="ER">
    Välkommen till Elins Raceturang
</h1>
</header>
<main> 
    <div id="burgare">
        <h2> Välj en hamburgare</h2>
        <p> Här väljer du din meny</p>
        <div class="wrapper">
          <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurgers="addToOrder($event)"/>
    </div>
  </div>

    
    <section id ="kundinfo" >
    <form>
        <h2> Kundinformation</h2>
        <p> Fyll i nödvändig information</p>
        <p>
            <label for="namn">Namn</label><br>
            <input type="text" id="namn" v-model="n" required="required" placeholder="För- och efternamn">
        </p>
        <p>
            <label for="email">E-mail</label><br>
            <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        </p>
       <!---- <p>
            <label for="Gatuadress">Gatuadress</label><br>
            <input type="text" id="street" v-model="str" required="required" placeholder="Gata">
        </p> 
        <p>
            <label for="husnummer">Husnummer</label><br>
            <input type="number" id="husnummer" v-model="husnum" required="required" placeholder="Husnummer">
        </p>  -->

        <p>
            <label for="betalning">Betalningsmetod</label><br>
            <select id="betalning" v-model="bet">
                <option >Kort</option>
                <option selected="selected">Swish </option>
                <option>Klarna</option>
                <option>Kontant</option>
            </select>
         </p>
         <p>
            <label for="kön">Könsidentitet</label><br>

            <input type="radio" id="kvinna" v-model="kön" value="kvinna" required="required" >
            <label for="kvinna">Kvinna</label><br>
            
            <input type="radio" id="man" v-model="kön" value="man" required="required" >
            <label for="man">Man</label><br>

            <input type="radio" id="annat" v-model="kön" value="annat" required="required" >
            <label for="annat">Annat</label><br>
           
            <input type="radio" id="ej" v-model="kön" value="ej" required="required" >
            <label for="ej">Vill ej ange</label><br>

        </p>
        <button id="knapp" v-on:click="printOrder" type="submit">
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTVAsFWo5zGBasUSQbhAmqLZL4_GsU1gxB_dw&usqp=CAU"
                    alt = order title = order style="width:65px;height:50px;">
                    Skicka beställning
        </button>
    </form>
    </section>

    <div id="mapId">
      <div id="map" v-on:click="setLocation">
        <div v-bind:style="{left:location.x + 'px', top:location.y + 'px'}">
          T
      </div>
    </div>
  </div>
</main>
<footer>
    <hr>
    <div id="copyright">
    &copy 2022 Elins Raceturang AB
  </div>
</footer>

  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/*let MenuItem = function(nm,url,kr,gl,lac){
  this.name = nm;
  this.picture = url;
  this.price = kr;
  this.gluten = gl;
  this.lactose = lac;
}

let burgers = [new MenuItem("Lewis Halloumilton", "https://www.max.se/contentassets/bd97342be78047eb8f8026452d545538/product_halloumiburgare2.png?width=1160&sharpen=5&sigma=1,4&threshold=0", 99, true, true),
                new MenuItem("Lando No ris", "https://www.max.se/contentassets/1fdf0ed6657c4de8a5a89751b81bb875/product_hamburgare.png?width=1160&sharpen=5&sigma=1,4&threshold=0", 75, true, false),
                new MenuItem("Carlos Heinz", "https://www.max.se/contentassets/2c64a1d06cc64c9284fbfbca16d37f59/product_cheeseburgare.png?width=1160&sharpen=5&sigma=1,4&threshold=0", 99, true,true)
]
*/



export default {
  name: 'HomeView',
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: menu,
      n:'',
      em:'',
      bet:'',
      kön:'',

      orderedBurgers: {},
      location: { x: 0,
            y: 0
          }
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
                 this.location.x=event.clientX-10-offset.x;
                 this.location.x=event.clientY-10-offset.y;

    },
    printOrder: function(){
      console.log(this.n, this.em, this.bet, this.kön)
      console.log(this.orderedBurgers)
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y
        },
        orderItems: this.orderedBurgers,
        customerInformation: [this.n, this.em, this.bet, this.kön]

    });

  },

  addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
    },

  setLocation: function(event){
    this.location.x=event.clientX - 10 - event.currentTarget.getBoundingClientRect().left;
    this.location.y=event.clientY - 10 - event.currentTarget.getBoundingClientRect().top;
  }

  }
}
</script>

<style>
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
body{
    font-family: sans-serif;
    background-color: rgb(32, 32, 32);
}

.allergi{
    color: #560a0a;
    text-transform: uppercase;
 }
 
 #burgare {
     background-color: #c51313;
     color: white;
     margin: 30px;
     border: 2px Dashed white;
     padding: 5%;
 }

 #kundinfo{
    margin: 30px;
    border: 2px Dashed white;
    padding: 2%;
    background-color: #c51313;
 }

 button:hover {
    background-color: black;
    color: white;

 }

#knapp{
    cursor: grab;
    margin-top: 25px;
    margin-bottom: 25px;

 }

 div{
    padding-right: 5px;
    padding-left: 5px;
 }

 .front {
    margin: 30px;
    overflow:hidden;
 }

 #bild1{
    opacity: 0.8;
    width: 100%;
    height: 450px;
 }

 #burgarebild{
    height: 300px;
    width: 320px;
 }

 #ER {
    -webkit-text-stroke: 1px black;
    font-family: Tahoma; color: rgb(255, 221, 0);

    position: absolute;
    padding: 5%;
    margin-top: -35%;

 }

 #copyright{
    color:white;
 }

 .wrapper{
   display: grid;
   grid-gap: 2%;
   grid-template-columns: 33% 33% 33%;

 }


 #mapId{
  overflow: scroll;
  margin-left: 1.2em;
  width: 500px;
  height: 500px;
  position: relative;
 }

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/Users/elineckervald/Documents/grännssnitt/1md031-lab-2022/public/img/polacks.jpg");
    cursor: pointer;
    position: absolute;

  }
  #map:hover{
    background-color: pink;
  }

  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
</style>