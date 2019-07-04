<template>
  <div class = "container">
      <div class = "temp-section">
        <h3>{{temperature}} &#8451;</h3>
        <h3>{{location}}</h3>
      </div>
    <div class="date-section">
      <h1>{{hour}}:{{minute}}</h1>
      <div v-if = "name.length === 0">
        <h3>What is Your name?</h3>
        <input type="text" v-model = "nameInputDetection" v-on:keyup.enter="storeName"> 
      </div>
      <div v-else>
        <h3 style = "display:inline;">Good {{message}}, </h3><h3 style = "display:inline;">{{name}}. </h3>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data () {
    return {
      hour: new Date().getHours(),
      minute: new Date().getMinutes(),
      message: '',
      name: '',
      nameInputDetection: '',
      temperature: '',
      location: '',
    }
  },
  created(){
    let rand = parseInt(Math.random() * 5 + 1);
    document.getElementById('image').src = `../icons/backgrounds/${rand}.png`;
    this.getLocation();
  },
  mounted(){
    if(0 <= this.hour < 12){
      this.message = "Morning"
    }
    else if( 6 > this.hour >= 12 ){
      this.message = "Afternoon"
    }else{
      this.message = "Evening"
    }

    if(this.minute < 10){
      this.minute = '0' + this.minute
    }

    if(localStorage.name) this.name = localStorage.name;
  },
  methods:{
    storeName(){
      this.name = this.nameInputDetection;
    },
    getLocation(){
    if('geolocation' in navigator){
      var vm = this; 

      navigator.geolocation.getCurrentPosition(
        function success(position){
             axios.get(`http://api.apixu.com/v1/current.json?key=3c58889021c44dc394c170421190407&q=${position.coords.latitude},${position.coords.longitude}`)
             .then(res => {
              vm.temperature = res.data.current.feelslike_c;
              vm.location = res.data.location.name;
              console.log(vm.temperature);
              })
             .catch(err => console.log(err));
        },
        function error(error_message){
           console.error('An error has occured while retrieving location', error_message)
        }
      );
    }else{
      console.log('geolocation is not enabled on this browser');
    }
    }
  },
  watch: {
    name(newName) {
      localStorage.name = newName;
    }
  }
}
</script>

<style scoped>
div{
  font-family: 'Poppins', sans-serif;
}
.container{
  position: absolute;
}
.date-section{
  font-size: 75px;
  color: white;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  position: fixed;
  align-items: center;
  justify-content: center;
}
h1, h3{
  margin: 0;
}
input{
  background: transparent;
  border:none;
  border-bottom: 10px solid #ffffff;
  height: 60px;
  color: white;
  font-size: 50px;
  width: 100%;
  font-weight: bold;
  text-align: center;
}
.temp-section{
  font-size: 25px;
  color: white;
  position: fixed;
  text-align: right;
  width: 99%;
}
</style>