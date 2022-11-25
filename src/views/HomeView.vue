<template>
  <div>
              
    <header> 
      <div class="header">
            <img src="https://i.pinimg.com/564x/fd/77/da/fd77da75332a9b8c8a6556e487bafc6d.jpg" id="headpic">
            <h1 id="headtext">Välkommen till krångliga krabban</h1>
      </div>
    </header>

        <section id="borgir" >
            <h1 class="burtext">
                Välj din Borgir
              </h1>
              <div class="burtext"> 
                Här väljer du vilken burgare du är sugen på yani
              </div>

              <div class="wrapper burre">
                <Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"
                v-on:orderedBurgers="addToOrder($event)"/>
              </div>
        </section>


          <section id="contact">
            <div class="ctext">
                <h2>
                Kundinfo
              </h2>  
              <div>
                Behöver lite info, din lilla gahba
              </div>  
          
              <form>
                    <p>
                        <label for="name">Namn</label><br>
                        <input type="text" id="name" v-model="n" required="required" placeholder="Namn">
                    </p>
                    <p>
                        <label for="email">E-mail</label><br>
                        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                    </p>
                   <!-- <p>
                        <label for="adress">Adress</label><br>
                        <input type="text" id="adress" v-model="ad" placeholder="Adress">
                    </p>
                    <p>
                        <label for="housenumber">Husnummer</label><br>
                        <input type="number" id="housenumber" v-model="nr" placeholder="Husnummer">
                    </p>-->

                    <p>
                        <label for="betalningsmetod">Betalningsmetod</label> <br> <!--fanns <be> innan br men funkade ej -->
                        <select id="payment" v-model="pay" >
                            <option>Swish</option>
                            <option selected="Kort">Kort</option>
                            <option>Klarna</option>
                            <option>Kontant</option>
                            <option>Natura</option>
                        </select>
                     </p>

                     <h3>
                        Kön
                    </h3>  
                        <input type="radio" id="kvinna" v-model="gender" value="kvinna">
                        <label for="kvinna">Kvinna</label><br>
                        <input type="radio" id="man" v-model="gender" value="man">
                        <label for="man">Man</label><br>
                        <input type="radio" id="vea" v-model="gender" value="vill ej ange" checked="checked">
                        <label for="vill ej ange">Vill ej ange/Annat</label>
                </form>
            </div>
            <div class="ctext">
            <h3>Klicka i din location bre</h3>
            <div id="map" v-on:click="setLocation">
                <div id="dot" :style="{left: location.x + 'px',top: location.y + 'px'}">
                </div>
            </div>
            </div>
            
        </section> 
            <br>


            <section id="submit">
                <button type="submit" v-on:click="addOrder" >
                    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRzBZShxkwLuCWvEWCEnu8XD9-uU8XLbx-c4Q&usqp=CAU" style="width:20px;height:20px;">
                    Beställ
                </button> 



            </section>             
 
            
   
    <hr>
    <footer>
        &#169; 2022 Copyright yeah
    </footer>

    </div>
</template>

<script>
  import Burger from '../components/OneBurger.vue'
  import io from 'socket.io-client'
  import menu from '../assets/menu.json'

  const socket = io();

  /*function menuItem(name,url,kcal,al){
    this.name=name;
    this.image=url;
    this.kcal=kcal;
    this.allergy=al;
  }
  

  let burger1= new menuItem("Krabb Burgaren","300","https://cdn.mos.cms.futurecdn.net/28Ltb9FTfcnKzLb5yfLsKi-320-80.jpg", "Inga");
  let burger2= new menuItem("Anti-Krabb Burgaren","300","pic", "lactos:nej gluten:nej"); 
  let burger3= new menuItem("Mini-Krabban","100","pic", "Söt jue");

  let allBurgers=[burger1,burger2,burger3];

  console.log(allBurgers);
  */

  export default {
    name: 'HomeView',
    components: {
      Burger
    },
    data: function () {
      return {
        burgers: menu,
        n:"",
        em:"",
        //ad:"",
        //nr:"",
        pay:"",
        gender:"",
        orderedBurgers:{},
        location: { x: 0, y: 0}
      }
    },
    methods: {
      printInformation: function(){
        console.log(this.n, this.em,/*this.ad, this.nr,*/this.pay,this.gender)
        //console.log(this.orderedBurgers)
      },
      getOrderNumber: function () {
        return Math.floor(Math.random()*100000);
      },
      addOrder: function () {
        socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                  details: { x: this.location.x,
                                            y: this.location.y },
                                  orderItems: this.orderedBurgers,
                                  personalInfo: {name: this.n, email: this.em, pay:this.pay, gender:this.gender} 
                                }
                  );
        this.printInformation()
      },
      setLocation: function (event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                      y: event.currentTarget.getBoundingClientRect().top};
        this.location.x = event.clientX - 10 - offset.x
        this.location.y = event.clientY - 10 - offset.y
      }
      ,
      //
      //addOrder: function (event) {
      //  var offset = {x: event.currentTarget.getBoundingClientRect().left,
      //                y: event.currentTarget.getBoundingClientRect().top};
      //  socket.emit("addOrder", { orderId: this.getOrderNumber(),
      //                            details: { x: event.clientX - 10 - offset.x,
      //                                      y: event.clientY - 10 - offset.y },
      //                            orderItems: ["Beans", "Curry"]
      //                          }
      //            );
      //},
      addToOrder:function(event){
        this.orderedBurgers[event.name]=event.amount;
        
      }
      
      //setLocation: function(event){

      //}
    }
  }
</script>

<style>
@import url('https://fonts.cdnfonts.com/css/spongebob-font-condensed');

  #map {
    width: 700px;
    height: 400px;
    position: relative;
    /*background-color: red;*/
    background: url("https://cdn.jwplayer.com/v2/media/WVSKsBQo/poster.jpg?width=720");
    cursor: crosshair;
    margin: 0;
    padding: 0;
  }
  
  #dot {
    position: absolute;
    background: red;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

  body {
    font-family: "Droid Serif", sans-serif;
    background-color: #edcbfd;
    margin-left: 20px;
 }

 .header {
   height: 300px;
   overflow:hidden;
   /*font-family: 'SpongeBob Font Condensed', sans-serif;*/
   font-family: 'SpongeBob Font Wide', sans-serif;
 }

 #headpic{
   opacity:50%;
   width: 100%;
   height: auto;
 }

 #headtext{  
   position:absolute; 
   margin-top: -500px;
   margin-left: 200px;
}

 .burre{
    padding: 40px;
 }

 .allergier {
    font-style: italic;
 }
 
 #borgir {
    background-color: rgb(232, 134, 200);
    border: 10px dotted orange;
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


 #contact{
    background-color: aquamarine;
    margin-top: 50px;
    margin-bottom: 40px;
    border: 10px solid #97f9ca
 }

 .ctext{
    margin-left: 20px;
    margin-bottom: 20px;
 }

 button:hover {
    color: green;
    background-color: greenyellow;
    cursor: pointer;
 }

 footer{
   margin-top: 20px;
 }
</style>