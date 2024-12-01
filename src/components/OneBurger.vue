<template>
  <div class="burgers">
    <div id="oneBurger">
      <h3>{{burger.name}}</h3>
      <img v-bind:src="burger.url" style="width:250px">
      <ul>
        <li>{{ burger.kCal }} kCal</li> 
        <li v-if="burger.gluten==true" class="bold">Gluten</li>
        <li v-if="burger.lactose==true" class="bold">Lactose</li>
      </ul>
    </div>
    <div id="orderedBurgers">
      <p>Amount ordered: {{ amountOrdered }}</p>
      <div id="buttons">
        <button v-on:click="addBurger">+</button>
        <button v-on:click="removeBurger">-</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function(){
    return{
      amountOrdered:0,
    }
  },
  name: 'OneBurger',
  props: {
    burger: Object
  },
  methods:{
    addBurger: function(){
      this.amountOrdered+=1
      this.$emit('orderedBurger',{
        name:this.burger.name,
        amount:this.amountOrdered
      });
    },
    removeBurger: function(){
      if (this.amountOrdered>0){
        this.amountOrdered-=1
        this.$emit('orderedBurger',{
        name:this.burger.name,
        amount:this.amountOrdered
      })
      }
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .burgers{
    margin:10px;
    padding: 5px;
    margin-left:auto;
    margin-right:auto;
    background-color: #6a507d;
    border-style: groove;
    border-width: 5px;
  }
  #oneBurger{
    height:350px;

  }
  #orderedBurgers{
    margin-top: 10px;
  }
  #buttons{
    margin-left: 40%;
  }
</style>