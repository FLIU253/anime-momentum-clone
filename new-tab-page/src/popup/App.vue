<template>
  <div>
    <h1 class = "title">Schedule</h1>
    <div class = "date-section">
      <span>Monday</span>
      <span>Tuesday</span>
      <span>Wednesday</span>
      <span>Thursday</span>
      <span>Friday</span>
      <span>Saturday</span>
      <span>Sunday</span>
    </div>
    
   <div>
      <div v-for= "(anime, i) in animeList" v-bind:key="i">
      {{anime.title}}
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
  }
}
</script>

<style lang="scss" scoped>
.title{
  width: 500px;
  text-align: center;
}
.date-section{
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
  margin: 10px;
}
</style>
