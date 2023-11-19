<template>
  <figure>
    <figcaption>{{ burger.name }} </figcaption>
    <img class="pics" v-bind:src="burger.url">

    <section id="information">
      <li> {{ burger.kCal }}</li>
      <li> {{ burger.gl }} </li>
      <li> {{ burger.la }}</li>
      <section class="buttons">
        <button v-on:click="decreaseBurger">-</button>
        {{ amountOrdered }}
        <button v-on:click="increaseBurger">+</button>

        <button class="bigbutton" v-on:click="addBurgers">Add to order</button>
      </section>
    </section>
  </figure>
</template>
  
<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0
    }

  },
  emits: ['orderedBurgers'],
  methods: {
    increaseBurger: function () {
      this.amountOrdered += 1
    },
    decreaseBurger: function () {
      if (this.amountOrdered != 0) {
        this.amountOrdered -= 1
      }
    },
    addBurgers: function () {
      this.$emit('orderedBurgers', {
        name: this.burger.name,
        amount: this.amountOrdered
      }
      );
      this.amountOrdered = 0;
    },
  }

}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
figcaption {
  font-weight: bold;
  text-align: start;
}

.pics {
  border: 10px double lightblue;
}

#information {
  font-family: 'Courier New', Courier, monospace;
  font-weight: bold;
  text-align: left;
}

button {
  width: 30px;
  height: 30px;
  position: relative;
  margin: auto;
  margin-top: 0%;
  font-family: 'Courier New', Courier, monospace;
  font-weight: bold;
}

.bigbutton {
  width: 100px;
  height: 40px;
  float: right;
  margin: auto;
  color: rgb(81, 79, 234);
}

li {
  font-family: 'Courier New', Courier, monospace;
  font-weight: bold;
  text-align: left;

}

.buttons {
  display: flex;
}</style>
  
