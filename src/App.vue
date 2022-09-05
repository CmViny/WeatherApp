<script>
  export default {
    name: 'App',
    data() {
      return {
        api_key: '3fa04a081d2f23dc948a3bbf5b9d9edb',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {},
      }
    },
    methods: {
      fetchWeather(e) {
        if(e.key == "Enter") {
          fetch(`${this.url_base}find?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
        }
      },
      setResults(results) {
        this.weather = results;
      },

      dateBuilder(){
        let date = new Date();
        return date.toLocaleDateString("en")
      }
    }
  }
</script>

<template>
  <div id="app"
    :class="weather.list[0].main.temp > 16 ?
    'warm' : ''"
  >

    <main>
      <!-- SEARCH BOX -->
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>

      <!-- WEATHER WRAP -->
      <div class="weather-wrap" v-if="query.length != 0">
        <!-- LOCATION BOX -->
        <div class="location-box">
          <div class="location"> {{weather.list[0].name}}, {{weather.list[0].sys.country}}</div> 
          <div class="date">{{dateBuilder()}}</div>
        </div>

        <!-- WEATHER BOX -->
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.list[0].main.temp)}}°c</div>
          <div class="weather">{{weather.list[0].weather[0].main}}</div>
        </div>

      </div>
    </main>

  </div>
</template>

<style>

  /* GENERAL */
  * 
  {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body 
  {
    font-family: 'montserrat', sans-serif;
  }

  #app 
  {
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm 
  {
    background-image: url('./assets/warm-bg.jpg');
  }

  main 
  {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }

  /* SEARCH BOX */
  .search-box
  {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar
  {
    display: block;
    width: 100%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus 
  {
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0px 16px 0px;
  }

  /* LOCATION BOX */
  .location-box .location 
  {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date 
  {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }

  /* WEATHER BOX */
  .weather-box
  {
    text-align: center;
  }

  .weather-box .temp 
  {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .weather
  {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);;
  }
</style>
