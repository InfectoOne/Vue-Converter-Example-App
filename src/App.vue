<!-- the root component of our app -->
<!-- the HTML template for this component: -->
<template>
  <div id="app">
   <Navbar @select-quantity="chosenQuantity=$event"/>  <!-- we place the Navbar.vue component here and listen to its select-quantity event here. When it appens, we change the chosenQuantity -->
   <Converter v-bind:physicalQuantity="chosenQuantity"/>  <!-- we place the Converter.vue component here and pass the chosen physical quantity to it as a prop -->
  </div> 
</template>


<!-- the logic of this component: -->
<script>
import Navbar from './components/Navbar.vue'            /* import some other components */
import Converter from './components/Converter.vue'

export default {
  name: 'app',

  components: {  /* list all other components which we use inside this component */
    Converter,
    Navbar
  },

  data: function(){
    return{
      chosenQuantity: {name:"Time",id:2, units:["s","min","h"]}         /* the currently chosen quantity. Per default, the chosen quantity is TIME */
    }
  },

  methods:{
    /* ... */
  },

  computed: {
    getMeasure(){
      return this.quantities.find(x => x.name==this.chosenQuantity);
    }
  }
}

</script>


<!-- some CSS styling. There is no "scoped" attribute, so this styling applies to the whole app -->
<style>

  body{
    margin:0;
    font-family:"Source Sans Pro";
  }

  #app{
    display:flex;
  }
</style>


