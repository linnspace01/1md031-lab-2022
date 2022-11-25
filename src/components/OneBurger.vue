<template>
 
  <div class="burgerMenu">
    <h2 class="burtext">
        {{burger.name}}
    </h2>
    <img class="burgerImage burre" v-bind:src="burger.img" > 
    <!-- <ul class="allergier" id="al">
        <li>Innehåller bannemig ALLT</li>
        <li>Så Jävla god</li>
        <li>KÖP</li>
    </ul>-->
    <div class="wrapper allergier"> 
      - {{burger.content}} 
      <br> <br> 
      - {{burger.content2}}</div> 
      <br>
    <p> Antal: {{ amountOrdered }}</p>
    <button v-on:click="addBurger(key)" id="add"> 
      Lägg till burgare
    </button>
    <button v-on:click="removeBurger(key)" id="remove"> 
      Ta bort burgare
    </button>
    {{orderedBurgers}}
  </div>
</template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
  

  data: function () {
  return {
    amountOrdered: 0,
  
  }
},

methods: {
  addBurger: function(){
    this.amountOrdered++;
    this.$emit('orderedBurgers', { name:   this.burger.name, 
                                amount: this.amountOrdered});
  },
  removeBurger: function(){
    if (this.amountOrdered !=0) {
      this.amountOrdered--;
      this.$emit('orderedBurgers', { name:   this.burger.name, 
                                amount: this.amountOrdered});
    }
  },
  }
}

  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

.burre{
    padding: 40px;
    width:200px;
    height:150px;
 }

 .allergier {
    font-style: italic;
 }

 .burtext{
    margin-left: 20px;
    color: #fff;
 }

 .wrapper {
   display: grid;
   grid-gap: 10px;
   grid-template-columns: auto auto auto;
   color: #fff;
}

.box {
   color: #fff;
   font-size: 100%;
   grid-template-columns: auto auto auto
}
  
  </style>
  
  