<template>
  <div class="home" :class="typeof weather.main !='undefined' && weather.main.temp > 30 ? 'warn' : ''">
    <main>
      <div class="search">
        <input type="text" class="search-bar"
         placeholder="Search.."
         v-model="query"
         @keypress="fetchWeather()"
          />
         
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}} , {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
    
  </div>
</template>

<script>
// @ is an alias to /src


export default {
  name: 'Home',
  data() {
    return {
      api_key:'bc57f017d5066faee7ad7791bae2e85b',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}
    }
  },
  methods:{
    dateBuilder(){
      let d=new Date();
      let months=["January", "February", "March",  "April", "May", "June", "July", "August", 
      "September", "October", "November", "December"];
      let days=["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday" ]
      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();
      let dateFormat=day +" "+ date+" "+ month+ " " + year;
      return dateFormat;
    },
    fetchWeather(e){
      if(e.keyCode === 13){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res =>{
          return res.json();
        }).then(this.setResults)
        
      }
    },
    setResults(results){
      this.weather = results;
      
    }
  },
  created(){
    window.addEventListener("keydown", this.fetchWeather)
  }
}
</script>

<style scoped>

.home{
  background-image: url('./../assets/weather.jpg');
  background-position: bottom;
  background-size: cover;
  transition: 0.4;
  height: 100%;
  position: fixed;
  width: 100%;
}
.home.warn{
  background-image: url('./../assets/as.jpg');
  background-position: bottom;
  background-size: cover;
  transition: 0.4;
  height: 100%;
  position: fixed;
  width: 100%;
}
main{
  position: relative;
  height: 100%;
  padding:  20px 2%;
  width: 100%;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.45));
}
.search{
  width: 100%;
  height: 100%;
  margin-bottom: 38px;
  position: relative;
}
.search .search-bar{
  position: relative;
  display: block;
  width: 96%;
  height: 45px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  outline: none;
  border: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
}
.weather-wrap{
  position: relative;
  top: -97%;
}
.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 4px 6px rgba(0, 0, 0, 0.35);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 4px 6px rgba(0, 0, 0, 0.35);;
}
.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 4px 6px rgba(0, 0, 0, 0.35);
}
</style>
