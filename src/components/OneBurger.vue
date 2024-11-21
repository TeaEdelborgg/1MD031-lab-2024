<template>
  <div class="burgers">
    <p>Amount ordered: {{ amountOrdered }}</p>
    <button v-on:click="addBurger">+</button>
    <button v-on:click="removeBurger">-</button>
    <h3>{{burger.name}}</h3>
    <img v-bind:src="burger.url" style="width:150px">
    <ul>
      <li>{{ burger.kCal }} kCal</li> 
      <li v-if="burger.gluten==true" class="bold">Gluten</li>
      <li v-if="burger.lactose==true" class="bold">Lactose</li>
    </ul>
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
      this.amountOrdered-=1
      this.$emit('orderedBurger',{
        name:this.burger.name,
        amount:this.amountOrdered
      })
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .burgers{
    margin:10px;
    padding: 5px;
  }
</style>