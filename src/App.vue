<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data(){
    return{
      api_key: '560bcb863a054fc336cee1a4b6bd9c86',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let currentDate = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October" , "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let date = currentDate.getDate();
      let day = days[currentDate.getDay()];
      let month = months[currentDate.getMonth()];
      let year = currentDate.getFullYear();

      return `${day} ${date} ${month} ${year}`;
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
  background-color: rgb(15, 15, 15);
}

#app{
  background-image: url('./assets/cold-image.jpg');
  background-size: cover;
  background-repeat: none;
  background-position: center;
  transition: 500ms;
}
#app.warm{
  background-image: url('./assets/warm-image.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #5f5f5f;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0 0 8px rgba(0,0,0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 25px 0 25px;
  transition: 300ms;
}
.search-box .search-bar:focus{
  box-shadow: 0 0 16px rgba(0,0,0, 0.25);
  background-color: rgba(0,0,0, 0.75);
  border-radius: 25px 0 25px 0;
}

.location-box .location{
  color: #ffffff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0, 0.25);
}

.location-box .date{
  color: #ffffff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 25px;
  margin: 30px 0;
  box-shadow: 3px 5px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color: #ffffff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>