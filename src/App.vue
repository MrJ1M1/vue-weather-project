<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp < 16 ? 'cold' : '' ">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Enter your favorite country or city ..." v-model="query" @keypress="fetchWeather"/>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="weather-day">{{dayBuilt()}}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather-kind">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api_key: '8ec902ad7a367616ba11f252249a4e43',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e){
      if(e.key == 'Enter'){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults)
      }
    },
    setResults(results){
      this.weather = results;
      // console.log(this.weather);
    },
    dayBuilt(){
      let times = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      let days = ['Sunday','Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

      let day = days[times.getDay()];
      let date = times.getDate();
      let month = months[times.getMonth()];
      let year = times.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }

  },
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
    background-image: url('./assets/warm.jpg');
    background-size: cover;
    background-position: center;
    transition: 0.4s;
  }
  #app.cold{
    background-image: url('./assets/cold.jpg');
  }
  main{
    display: grid;
    grid-template-columns: 50% 50%;
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.407));
  }
  .search-box{
    width: 90%;

  }
  .search-box .search-bar{
    margin-top: 30vh;
    display: block;
    width: 100%;
    padding: 15px;
    color: #423f3f;
    font-size: 20px;
    border: none;
    background: none;
    outline: none;
    appearance: none;
    box-shadow: 0px 0px 9px rgba(0, 0, 0, 0.4);
    background-color: white;
    border-radius: 0px 18px 0px 18px;
    transition: 0.5s;
  }
  .search-box .search-bar:focus{
    box-shadow: 0px 0px 18px rgba(0, 0, 0, 0.4);
    background-color: rgba(255, 255, 255, 0.8);
    border-radius: 18px 0px 18px 0px;
  }
  .location-box .location{
    color: white;
    font-size: 32px;
    font-weight: 600;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .location-box .weather-day{
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }
  .weather-box{
    text-align: center;
  }
  .weather-box .temperature{
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.749);
    border-radius: 18px;
    margin: 30px 0;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
  .weather-box .weather-kind{
    color: #fff;
    font-size: 28px;
    font-style: italic;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
</style>