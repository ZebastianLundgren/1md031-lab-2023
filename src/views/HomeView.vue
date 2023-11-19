<template>
  <header>
    <h1>Welcome to Zebbe's Burgerz!</h1>
    <p>Order burgers straight to your door!</p>
  </header>

  <main>
    <section id="section1">
      <h2>Select your burger</h2>
      <div class="wrapper">
        <Burger v-for="burger in burgers" v-bind:burger="burger" v-bind:key="burger.name"
          v-on:orderedBurgers="addToOrder($event)" />


      </div>
    </section>
    <div id="mapcontainer">
      <div id="map" v-on:click="setLocation">
        <div id="dots">
          <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">T</div>
        </div>
      </div>
    </div>
    <section id="section2">
      <h2>Delivery information</h2>
      <p>
        <label for="Fullname">Full name</label><br>
        <input type="text" id="flname" v-model="fln" required="required" placeholder="First- and lastname">
      </p>
      <p>
        <label for="Email">E-Mail</label><br>
        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
      </p>
      <!--<p>
        <label for="Streetname">Street</label><br>
        <input type="text" id="Street" v-model="sn" required="required" placeholder="Streetname">
      </p>
      <p>
        <label for="Housenumber">House</label><br>
        <input type="number" id="Housenumber" v-model="hn" required="required" placeholder="Housenumber">
      </p>
-->
      <p>
        <label for="Payment options">Payment options</label>
        <br>
        <select id="recipient" v-model="rcp">
          <option checked="checked">Cash</option>
          <option>Card</option>
          <option>Swish</option>
          <option>Pay later</option>
        </select>
      </p>
      <form>
        <h3>Specify your gender:</h3>
        <label>
          <input type="radio" v-model="gender" value="male" checked="checked"> Male
        </label>
        <label>
          <input type="radio" v-model="gender" value="female"> Female
        </label>
        <label>
          <input type="radio" v-model="gender" value="other"> Other
        </label>
      </form>
      <button v-on:click="markDone" type="submit">
        
        <p>Place order</p>
      </button>
    </section>
  </main>

  <footer>
    <p>Zebbe's Burgerz Inc - ALL RIGHT RESERVED - For more information check out my instagram @zebastian_lundgren!</p>
  </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/*function MenuItem(nm, kc, url, gl, la) {
  this.name = nm;
  this.kCal = kc;
  this.url = url;
  this.gl = gl;
  this.la = la;
}

let allBurgers = [
  { name: "Cheeseburger", kCal: 500, url: "img/nedladdning.jpg", gl: true, la: false },
  { name: "Halloumiburger", kCal: 400, url: "img/nedladdning.jpg", gl: false, la: false },
  { name: "Chickenburger", kCal: 300, url: "img/nedladdning.jpg", gl: true, la: false }
]*/

console.log(menu);


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fln: "",
      em: "",
      gender: "",
      rcp: "",
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      }
    }

  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location.x = event.clientX - 10 - offset.x,
        this.location.y = event.clientY - 10 - offset.y,
        console.log(this.location.x)
      console.log(this.location.y)

      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y

        },
        orderItems: this.orderedBurgers,

      }
      );
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location.x = event.clientX - 10 - offset.x,
        this.location.y = event.clientY - 10 - offset.y,
        console.log(this.location.x)
      console.log(this.location.y)
    },

    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;

    },
    markDone: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y

        },
        orderItems: this.orderedBurgers,
        orderInfo: {
          Name: this.fln,
          Email: this.em,
          Gender: this.gender,
          Payment: this.rcp
        }
      },
        console.log(this.orderedBurgers),
        this.fln = "",
        this.em = "",
        this.rcp = "",
        this.orderedBurgers = ""

      );
    }
  }
}
</script>

<style>
@font-face {
  font-family: myFirstFont;
  src: url("C:\Users\zebas\Documents\1md031-lab-2023\public\img\MADEMountain-Regular.otf");

}

body {
  font-family: myFirstFont;
  font-size: 12pt;
  background-image: url("C:\Users\zebas\Documents\1md031-lab-2023\public\img\pattern.png");
  background-size: 10%;
  background-attachment: local;
}

button {
  display: block;
  width: 4em;
  margin-bottom: 1em;
}

#section1 {
  background-color: black;
  color: aliceblue;
  border: 10px double lightblue;
  
}

#section2 {
  background-color: black;
  border: 10px double lightblue;
}

h3, h2, label {
  color: rgb(81, 79, 234);
  font-size: large;
}

header {
  color: rgb(81, 79, 234);
  font-size: 2em;
  font-family: myFirstFont;
  font-weight: bold;
  margin-top: 3cm;
  margin: 2%;
  background-image: url("C:\Users\zebas\Documents\1md031-lab-2023\public\img\monster.png");
  background-size: 10%;
  background-attachment: local;
  text-align: center;
}

section {
  margin: 5%;
  padding: 1%;
}

button {
  margin-top: 5%;
  width: 100px;
}

footer {
  background-color: black;
  border: 10px double lightblue;
}

.wrapper {
  display: grid;
  grid-gap: 5px;
  grid-template-columns: repeat(3, 1fr);
  font-size: larger;

}

.box {
  border-radius: 1px;
  padding: 5px;
  margin: auto;
}

#map {
  background: url("C:\Users\zebas\Documents\1md031-lab-2023\public\img\polacks.jpg");
  width: 1920px;
  height: 1078px;


}

#mapcontainer {
  width: 100%;
  height: 60vh;
  overflow: scroll;
  border: 10px double lightblue;
  background-color: black;
}

h2 {
  font-size: 2em;
  margin: 0%;
  font-weight: bold;

}</style>