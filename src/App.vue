<template>
  <div id="app" :class="typeof weather.main !='undefined' && weather.main.temp > 23 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" 
        class="search-bar"
         placeholder="search..."
         v-model="query"
         @keypress="fetchWeather"
         
         />
         
      </div>
      <div v-if="seen" >
     <h1> Please Enter a location in the search bar.  </h1>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
 <div class="location-box">

 <div class="location"> {{ weather.name }}, {{ weather.sys.country }}</div>
  <div class="date">{{ dateBuilder() }}</div>
 </div>
<div class="weather-box">
<div class="temp"> {{ Math.round(weather.main.temp) }} </div>
 <div class="weather">{{ weather.weather[0].main }}</div>
</div>
 

      </div>

    </main>
     
  </div>
</template>

<script> 

export default {
  name: 'App',
 data () {
   return{
     api_key : '528c7cfcec5cca84b5830d14ee776aa5',
     url_base : 'https://api.openweathermap.org/data/2.5/',
     query:'',
     weather:{},
     seen : true
   }
 },
 methods : {
   fetchWeather (e) {

     if (e.key == "Enter") {
fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
.then(res=>{
  return res.json()
  
}).then(this.setResults);
     }
   },
   setResults (results) {
     this.weather = results;
this.seen = false;
   },
   dateBuilder (){
       let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
   
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
   
   }
   ,
   showxhide (e) {
if (e.key == "Enter") {
this.$emit('notify');
}

   }
 }
  
}
</script>

<style>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;

}

h1 {
  color: #fff;
  font-family: sans-serif;
  font-style: italic;
}

body{
  font-family: sans-serif ;
}
#app {
background-image: url('./assets/cold-bg.jpg');
background-size:cover ;
background-position: bottom;
transition: 0.4s;
}

#app.warm {
background-image: url('./assets/warm-bg.jpg');

}

main{
  min-height: 100vh;
  padding: 25px;

background-image: linear-gradient(to bottom, rgba(0,0,0,0.35), rgba(0,0,0,0.75));
}

.search-box {
  width:100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: black;
  font-size: 19px ;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: rgb(255, 255, 255,0.5) ; 
}

.location-box .location {
  color: #fff;
  font-size: 32px;
font-weight: 500;
text-align: center;

}

.location-box .date {
   color: #fff;
  font-size: 30px;
font-weight: 300;
text-align: center;
font-style: italic;
}

.weather-box {
  text-align-last: center;
}

.weather-box .temp {
display: inline-block;
padding: 10px 25px;
color: white;
font-size: 100px;
font-weight: 900; 
background-color: rgba(255,255, 255, 0.25);
border-radius: 15px;
margin: 25px;
box-shadow: 3px 6px rgba(0,0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 47px;
  font-weight: 500;
  font-style: italic;
  box-shadow: 3px 6px rgba(0,0, 0, 0.25);
}
</style>
