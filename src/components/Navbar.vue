<!-- this component represents the left navigation sidebar -->
<!-- the HTML-template for this component: -->
<template>
 <div id="nav">
     <div id="header">
         <img src="../assets/logo.png" height="30" width="30">
         Vue Converter 
    </div>
     <a v-for="quantity in quantities" v-bind:key="quantity.name" @click="$emit('select-quantity',quantity)" > <!-- list all quantities as navitems and when you click on a navitem, a custom event named 'select-quantity' is emitted. We listen to the event in the parent element App.vue -->
        {{quantity.name}}
     </a>
     <div id="footer">
        Vue.js converter by
        <img src="../assets/markoz_logo_white.png" height="46" width="126">
        www.markozunic.com
     </div>
 </div>
</template>


<!-- the logic for this component: -->
<script>
export default {
  name:'Navbar',
  data: function(){
      return{
          quantities: [  /*  all physical quantities we are working with and their respective units */
           {name:"Length",id:0, units:["mm","cm","m"]},
           {name:"Temperature",id:1, units:["C","K","F"]},
           {name:"Time",id:2, units:["s","min","h"]}
          ]
      }
  },
  methods:{
   /* ...... */
  }
}
</script>



<!-- CSS styling for the navbar. The "scoped" attribute limits the styling exclusively to THIS component -->
<style scoped>
    #nav{
        display:flex;
        flex-direction:column;
        height:100vh;
        width:20vw;
        background-color:rgb(53, 73, 94);
        color:white;
    }


    #nav>div{
        display:flex;
        flex-direction:column;
        justify-content:center;
        align-items:center;
        padding:20px 0 20px 0;
        text-align:Center;
        vertical-align:middle;
    }

    #header>img{
       background-color:white;
       padding:10px;
       border-radius:20%;
       margin: 5px;
    }

    a{
       text-align:center;
       padding:20px;
       background-color:rgb(77, 186, 135);
       transition:background-color 0.5s;
       cursor:pointer;
    }
    a:hover{
        background-color:rgb(57, 166, 125);
    }

    #footer{
        height:15vh;
        background-color:rgb(33, 53, 74);
        margin-top:auto;  /* places the footer at the end of the navbar */
        box-sizing:border-box;
        padding:15px;
        font-size:0.8em;
    }



</style>