<template>
  <div class="picblock">
    <h4>{{burger.name}}</h4>
    <img v-bind:src=burger.url1 class="chili">
    <img v-bind:src=burger.url2 class="burstrl">
    <ul>
      <li>{{burger.kcal}} kcal</li>
      <li v-if="burger.lactose">Contains <span class="boldie">lactose</span></li>
      <li v-if="burger.gluten">Contains <span class="boldie">gluten</span></li>
      <li v-if="burger.other" class="dogreen">Vegan</li>
    </ul>
    <button type="button" v-on:click="subFromOrder">v</button>
    {{amountOrdered}}
    <button type="button" v-on:click="addToOrder">^</button>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data:function(){
    return {
      amountOrdered: 0,
      orderedBurgers: {}
    }
  },
  methods: {
    subFromOrder: function () {
      if(this.amountOrdered>0){
        this.amountOrdered -=1,
        this.$emit("orderedBurger",
        {
          name:this.burger.name,
          amount: this.amountOrdered
        }
        );
      }
      else {
        this.amountOrdered =0;
      }
    },
    addToOrder: function () {
      this.amountOrdered +=1,
      this.$emit("orderedBurger",
      {
        name: this.burger.name,
        amount: this.amountOrdered
      }
      );
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import 'https://fonts.googleapis.com/css2?family=Shadows+Into+Light&display=swap';

/*Body*/
body {
	font-family: 'Shadows Into Light', cursive;
     }
/*Section*/
.cumstomersection {
	margin: 5px 0px;
	padding: 10px;
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

.burger1 {
	grid-column: 1;
}

.burger2 {
	grid-column: 2;
}

.burger3 {
	grid-column: 3;
}
/*Färger*/
.dogreen {
	color: #00ff00;
}

.doblack {
	color: #000000;
}

.doblackbackground {
	background-color: #000000;
}
.transparent {
	opacity: 0.5;
}
/*Texttyper*/
.boldie {
	font-weight: bold;
}

.chili {
  width: 100px;
}

.burstrl {
  width: 350px;
  height: 300px;
}
/*Blocking*/
.picblock img{
	display: block;
}
</style>
