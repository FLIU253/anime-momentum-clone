<template>
  <div>
    <h1 class = "title">Anime Schedule</h1>
    <div class = "date-section">
      <span  v-on:click ="getSchedule('monday')" style = "cursor: pointer;">Monday</span>
      <span  v-on:click ="getSchedule('tuesday')" style = "cursor: pointer;">Tuesday</span>
      <span  v-on:click ="getSchedule('wednesday')" style = "cursor: pointer;">Wednesday</span>
      <span  v-on:click ="getSchedule('thursday')" style = "cursor: pointer;">Thursday</span>
      <span  v-on:click ="getSchedule('friday')" style = "cursor: pointer;">Friday</span>
      <span  v-on:click ="getSchedule('saturday')" style = "cursor: pointer;">Saturday</span>
      <span  v-on:click ="getSchedule('sunday')" style = "cursor: pointer;">Sunday</span>
    </div>
    
   <div class= "anime-section">
      <div v-for= "(anime, i) in animeList" v-bind:key="i" class ="anime-card">
       <p class = "anime-title">{{anime.title}}</p>
       <a :href="anime.url" target = "_blank" style = "cursor: pointer;"><img :src="anime.image_url" alt=""></a>
    </div>
   </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data () {
    return {
      date: new Date(),
      daysObj: {'0' : 'sunday',  '1': 'monday' , '2': 'tuesday', '3': 'wednesday', '4' : 'thursday', '5': 'friday', '6': 'saturday'},
      animeList: [],
    }
  },
  created(){
   
  },
  mounted(){
     axios.get(`https://api.jikan.moe/v3/schedule/${this.daysObj[this.date.getDay()]}`)
    .then(res => {
      this.animeList = res.data[`${this.daysObj[this.date.getDay()]}`];
      console.log(this.animeList);
    }).catch(err => console.log(err));
  },
  methods:{
    getSchedule: function(date){
     axios.get(`https://api.jikan.moe/v3/schedule/${date}`)
      .then(res => {
        this.animeList = res.data[`${date}`];
        console.log(this.animeList);
      }).catch(err => console.log(err));
    }
  }
}
</script>

<style lang="scss" scoped>
div{
  //font-family: 'Merriweather', serif;
  font-family: 'Poppins', sans-serif;
  //font-family: 'Russo One', sans-serif;
  margin: 0;
  padding: 0;
}
.title{
  width: 500px;
  text-align: center;
  background-color: #59D375 ;
  margin: 0;
}
.date-section{
  display: grid;
  color: white;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
  background-color:#27201F;
  padding: 10px;
}
.anime-section{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-column-gap: 10px;
  grid-row-gap: 10px;
  text-align: center;
  background-color:#27201F;
  padding: 15px;
}
.anime-card{
  text-align: center;
  background-color: #EB7C61 ;
  padding-bottom: 10px;
}
.anime-title{
  max-height:20px;
  max-width: 150px;
  white-space: nowrap; 
  overflow: hidden;
  text-overflow: ellipsis;
}
img{
  height: 150px;
  width: 115px;
}
</style>
