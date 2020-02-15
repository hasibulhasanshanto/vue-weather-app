<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm': ''">
    <main class="search-box">
      <input 
        type="text" 
        class="search-bar" 
        placeholder="Search by City..."
        v-model="query"
        @keypress="fetchWeather"
      >
      <!-- <p>Made with Veu.js by -Hasibul Hasan</p> -->

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp )}}&#176;C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      
    </main>
    
  </div>
</template>

<script> 

export default {
  name: 'App',
  data (){
    return {
      api_key: '013ab712fd4fb65b144d08d6406e437a',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}

      // https://openweathermap.org/city
    }
  },
  methods: {
    fetchWeather(e){
      if (e.key =="Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then( data => {
            return data.json();
          }).then(this.setResults);
      }
    }, 
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wenesday', 'Thrusday', 'Friday', 'Satuday'];
      let day= days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month}, ${year}`;
    }
  } 
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}

#app{
  background-image: url('./assets/img/cold-bg.jpg');
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}
#app.warm{
  background-image: url('./assets/img/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0, 0.25), rgba(0,0,0, 0.75));
}
.search-box{
  width: 100%;
  
  
}

.search-box .search-bar{
  display: block; 
  margin : 0 auto;
  padding: 15px;
  min-width: 270px;
  margin-bottom: 50px;

  color:#313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255, 0.5); 
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.5);
  background-color: rgba(255,255,255, 0.75);
  border-radius: 16px 0px 16px 0px;
  transition: 0.4s;
}
.location-box .location{
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: #FFF;
  font-size: 22px;
  font-weight: 300;
  text-align: center; 
  font-style: italic;
  margin-top: 10px;
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color:#FFF;
  font-size: 90px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255, 0.25);
  border-radius: 16px;
  margin: 30px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}
.weather-box .weather{
  color: #FFF;
  font-size: 42px;
  font-weight: 700;
  font-style: italic;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
