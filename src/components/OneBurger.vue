<template>
           <div>
                    
                    <h3>{{burger.name}}: {{burger.price}} kr</h3>
                    <button v-on:click=increase type="button"> + </button>
                    <button v-on:click=decrease type="button"> - </button>
                    <h4>Antal beställda: {{ amountOrdered }}</h4>
                    
                        <img id="burgarebild" v-bind:src="burger.picture">

                        <ul>
                          <li v-if="burger.name=='Lewis Halloumilton'"> Lakto-ovo vegetarisk</li>
                          <li v-if="burger.name=='Lando No ris'"> Serveras utan ris</li>
                          <li v-if="burger.name=='Carlos Heinz'"> Extra mycket Heinz ketchup</li>
                          <li v-if="burger.gluten"> Innehåller <span class="allergi"> gluten</span></li>
                          <li v-if="burger.lactose"> Innehåller <span class="allergi"> laktos</span></li>
                        </ul>
    
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
    addBurger: function () {
    this.$emit('orderedBurgers', { name:   this.burger.name, 
                               amount: this.amountOrdered 
                              })
},
    increase: function(){
      this.amountOrdered +=1
      this.$emit('orderedBurgers', { name:   this.burger.name, 
                               amount: this.amountOrdered 
                              }
  );
    },

    decrease: function(){
      if(this.amountOrdered>0){
      this.amountOrdered -=1
      this.$emit('orderedBurgers', { name:   this.burger.name, 
                               amount: this.amountOrdered 
                              }
  );
      }
    },
  }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
   #burgarebild{
   height: 300px;
   width: 320px;
 }


 #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  </style>
  