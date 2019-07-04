<template>
  <div class = "container">
    <div class="date-section">
      <h1>{{hour}}:{{minute}}</h1>
      <h3>Good {{message}}.</h3>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      hour: new Date().getHours(),
      minute: new Date().getMinutes(),
      message: ''
    }
  },
  created(){
    let rand = parseInt(Math.random() * 5 + 1);
    document.getElementById('image').src = `../icons/backgrounds/${rand}.png`;

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
  }
}
</script>

<style scoped>
div{
  font-family: 'Poppins', sans-serif;
}
.container{
  position: relative;
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
</style>