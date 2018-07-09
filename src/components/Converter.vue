<!-- the main component of the app, where the actual converting happens -->
<!-- the HTML template of this component -->
<template>
  <div id="container">

    <h1>{{physicalQuantity.name}}</h1>

    <div id="main">
        <input type="text" v-model="fromValue" placeholder="enter amount">
        <select v-model="fromUnit">   <!-- choose unit to convert from -->
            <option v-for="unit in physicalQuantity.units" v-bind:key="unit"> {{unit}} </option>
        </select>
        <svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" viewBox="0 0 24 24" fill="rgb(77, 186, 135)"><path d="M14 12l-14 9v-18l14 9zm-4-9v4l8.022 5-8.022 5v4l14-9-14-9z"/></svg>
        <p id="result">
            {{result}}
        </p> 
        <select v-model="resultUnit" placeholder="result">   <!-- choose unit to convert to -->
            <option v-for="unit in physicalQuantity.units" v-bind:key="unit"> {{unit}} </option>
        </select>
    </div>

  </div>
</template>

<!-- the logic of this component -->
<script>
export default {
  name: 'Converter',
  
  props: {
    physicalQuantity: Object   // props is basically just data passed from a parent to a child component. App.vue is our parent here and it passes an Object to this variable
  },

  data: function(){            // the data available to this component (Converter.vue). It must be a function returning an object (to ensure that each component has its own data)
       return{
         fromValue:1,
         fromUnit:"",
         resultUnit:""
       }
  },

  methods:{
      // for each physical quantity, we have an "intermediary" unit and a function for converting values to that unit. That means, when we e.g. convert milimeters to centimeters, we don't do that directly
      // instead, we convert to meters first, and then to centimeters. Makes the programming logic more readable and maintanable. The following functions are used for converting to those intermediary units:

      toMinutes: function(value,unit){   // converts a time value to its corresponding value in minutes
          switch(unit){
            case("s"):
                return value/60;
            case("min"):
                return value;
            case("h"):
                 return value*60;
            default:
                 console.log("toMinutes conversion failed - unit incorrect")
                 return value;
          }
      },

      toMeters: function(value,unit){   // converts a length value to its corresponding value in meters
        switch(unit){
            case("mm"):
                return value/1000;
            case("cm"):
                return value/100;
            case("m"):
                return value;
            default:
                console.log("toMeters conversion failed - unit incorrect")
                return value;
        }
      },

      toCelsius: function(value,unit){   // converts a temperature value to its corresponding value in celsius
        switch(unit){
            case("C"):
                return value;
            case("F"):
                return (value-32)*5/9;
            case("K"):
                return value-273.15;
            default:
                console.log("toCelsius conversion failed - unit incorrect")
                return value;
        }
      }
  },


  computed: {            // "computed" parts of components are reactive values which depend on the value of some other part (in our case, the "fromValue")

      result: function(){
            let value = parseFloat(this.fromValue);  // whatever the user types into the <input> Element, it turns out to be a string. We have to convert it to a number(float) 
            if(isFinite(value)){                   // check if the user really typed in a number (if not, then isFinite returns false)        

              switch(this.physicalQuantity.name){    // check in which physicalQuantity we are
                case("Time"): {
                    /* ------------T I M E-------------*/
                        let valueInMinutes = this.toMinutes(value,this.fromUnit);  // we use "minutes" as an intermediary unit
                        switch(this.resultUnit){
                        case("s"):
                            return parseFloat((valueInMinutes*60).toFixed(5));   // limit to 5 decimals and parseFloat() it to remove redundant 0s
                        case("min"):
                            return parseFloat((valueInMinutes).toFixed(5));
                        case("h"):
                            return parseFloat((valueInMinutes/60).toFixed(5));
                        default:
                            console.log("Error while converting time - resultUnit not detected!");
                            return "...";
                         }
                }
                  /* ----------------------------*/
                 case("Length"): {
                    /* ------------L E N G T H-------------*/
                        let valueInMeters= this.toMeters(value,this.fromUnit);  // we use "meters" as an intermediary unit
                        switch(this.resultUnit){
                        case("mm"):
                            return parseFloat((valueInMeters*1000).toFixed(5));   
                        case("cm"):
                            return parseFloat((valueInMeters*100).toFixed(5));
                        case("m"):
                            return parseFloat((valueInMeters).toFixed(5));
                        default:
                            console.log("Error while converting length - resultUnit not detected!");
                            return "...";
                        }
                 }
                  /* ----------------------------*/
                  case("Temperature"): {
                    /* ------------T E M P E R A T U R E-------------*/
                        let valueInCelsius= this.toCelsius(value,this.fromUnit);  // we use "celsius" as an intermediary unit
                        switch(this.resultUnit){
                        case("C"):
                            return parseFloat((valueInCelsius).toFixed(5));   
                        case("K"):
                            return parseFloat((valueInCelsius+273.15).toFixed(5));
                        case("F"):
                            return parseFloat((valueInCelsius*1.8+32).toFixed(5));
                        default:
                            console.log("Error while converting temperature - resultUnit not detected!");
                            return "...";
                        }
                  }
                  /* ----------------------------*/

              }
              
            }      
            return "...";  // if the user didn't type in a number
      }
  }

}
</script>



<!-- CSS styling for this component. The "scoped" attribute limits the styling exclusibely to THIS component-->
<style scoped>

  #container{
    width:80vw;
    color:rgb(20,20,20);
    height:100vh;
    display:flex;
    flex-direction:column;
    justify-content:flex-start;
  }

  h1{
    width:100%;
    background-color:rgb(250, 250, 250);
    box-sizing:border-box;
    padding:40px;
  }

  #main{
    display:flex;
    box-sizing:border-box;
    padding:40px;
  }
  
  #main>*{
    margin:0 5px 0 5px;
  }

  input,select,#result{
    padding:10px;
    border: 1px solid rgb(77, 186, 135);
    border-radius:10px;
    width:10vw;
    font-family:"Source Sans Pro";
    font-size:1em;
    display:flex;
    align-items:center;
  }

</style>
