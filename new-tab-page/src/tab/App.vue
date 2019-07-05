<template>
  <div class = "container">
      <div class = "temp-section" style = "z-index: 9999">
        <div style = "text-align: left; margin-left: 20px; margin-top: 20px; cursor:pointer;">
          <span v-on:click="redirectToNewTab">Chrome Tab</span>
           <span v-on:click="redirectToApps" style = "margin-left: 20px;">Apps</span>
        </div>
        <div style = "text-align: center; margin-top: 20px;">
          <input type="text" class ="google-search" placeholder = "google search here!" v-model ="searchValue" v-on:keyup.enter="searchGoogle(searchValue)">
        </div>
      <div style = "margin-right: 20px; margin-top: 20px;">
        <h4>{{temperature}} &#8451;, {{weatherCondition}}</h4>
        <h4 style = "font-size: 20px; margin-top: 10px;">{{location}}</h4>
      </div>
      </div>
    <div class="date-section">
      <h1><i class="far fa-clock" style = "font-size: 90%;"></i> {{hour}}:{{minute}}</h1>
      <div v-if = "name.length === 0">
        <h3>What is Your name?</h3>
        <input type="text" v-model = "nameInputDetection" v-on:keyup.enter="storeName" class = "name-section"> 
      </div>
      <div v-else>
        <h3 style = "display:inline;">Good {{message}}, </h3><h3 style = "display:inline;">{{name}}. </h3>
      </div>
      <div class ="sauce-name">
        Sauce: {{picSauce.sub_category}}
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
      weatherCondition: '',
      searchValue: '',
      picSauce: '',
    }
  },
  created(){
    let rand = parseInt(Math.random() * 1000 + 1);
    let rand2 = parseInt(Math.random() * 30 + 1);

    axios.get(`https://wall.alphacoders.com/api2.0/get.php?auth=48652087b664d19d53d47976917858b8&method=category&id=3&sort=favorites&width=1920&height-1080&page=${rand}&info_level=3`,
    {
      headers: {
        "Content-Type": "application/json",
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Methods": "OPTIONS",
        "Access-Control-Allow-Headers": "Content-Type, Authorization",
        "Access-Control-Allow-Credentials": "true"
      }
    }
        )
    .then(res => {
        this.picSauce = res.data.wallpapers[rand2];
        document.getElementById('image').src = this.picSauce.url_image;
        console.log(this.picSauce.sub_category);
      })
    .catch(err => console.log(err));

    this.getLocation();
  },
  mounted(){
    if(0<= this.hour && this.hour < 12){
      this.message = "Morning";
    }
    else if( 13 <= this.hour && this.hour  < 18){
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
              vm.location = res.data.location.name + ', ' + res.data.location.region;
              vm.weatherCondition = res.data.current.condition.text;
              })
             .catch(err => console.log(err));
            
            axios.get(`http://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&units=metric&appid=2affd308270b35de26051eb43ed071fa`)
             .then(res => {
              vm.temperature = res.data.main.temp;
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
  },
  redirectToNewTab() {
       chrome.tabs.getCurrent(function(tab){
          chrome.tabs.update(tab.id, {
        url: "chrome-search://local-ntp/local-ntp.html?dev=false"
      });
    });
  },
  searchGoogle(value){
      chrome.tabs.getCurrent(function(tab){
          chrome.tabs.update(tab.id, {
        url: `https://www.google.com/search?q=${value}`
      });
    });
  },
  redirectToApps(){
     chrome.tabs.getCurrent(function(tab){
          chrome.tabs.update(tab.id, {
        url: `chrome://apps/`
      });
    });
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
 font-family: 'Lato', sans-serif;
}

.container{
  position: absolute;
}
.date-section{
  font-size: 65px;
  color: white;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  position: fixed;
  align-items: center;
  justify-content: center;
  mix-blend-mode: exclusion;
  text-align: center;
}
.google-search{
  background: transparent;
  border:none;
  border-bottom: 2px solid #ffffff;
  height: 30px;
  color: white;
  mix-blend-mode: exclusion;
  font-size: 20px;
  width: 100%;
  font-weight: bold;
  text-align: center;
}
::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: white;
  mix-blend-mode: exclusion;
  opacity: 0.6; /* Firefox */
}
h1, h3, h4{
  margin: 0;
}
h3{
  font-size: 40px;
}
.name-section{
  background: transparent;
  border:none;
  border-bottom: 5px solid #ffffff;
  height: 60px;
  color: white;
  mix-blend-mode: exclusion;
  font-size: 40px;
  width: 100%;
  font-weight: bold;
  text-align: center;
}
.temp-section{
  font-size: 25px;
  display:grid;
  grid-template-columns: repeat(3, 1fr);
  color: white;
  mix-blend-mode: exclusion;
  position: fixed;
  text-align: right;
  width: 99%;
}
.sauce-name{
  margin-top:  50px;
  font-size: 30px;
  bottom: 0;
}
</style>