<template>
    <div id="app">
    <main :class="getImageByTemp()">
      <div class="search_box d-flex">
        <input type="text" class="search-bar form-control" placeholder="Search City..." v-model="city"
          @keypress="fetchWeather">
        <box-icon name='search' color='#ffffff' class="search_icon"></box-icon>
      </div>
      <div class="weather-wrap" v-if="weather.location">
        <div class="location-box text-white">
          <div class="location text-center fw-bolder fs-2">{{ weather.location.name }} , {{ weather.location.country }}
          </div>
          <div class="date text-center fw-light fst-italic fs-4">{{ weather.location.localtime }}</div>
        </div>
        <div class="text-center ">
          <div class="degree mt-1">{{ weather.current.temp_c }}&deg;c / {{ weather.current.temp_f }}&deg;f</div>
          <div class="text-white mt-2 fs-2 fst-italic">{{ weather.current.condition.text }}</div>
        </div>
        <div class="d-flex text-center justify-content-center text gap-4 flex-wrap">
          <div class="degree1 mt-3"><small class="fs-4 fw-bold fst-italic">UV </small>{{ weather.current.uv }}</div>
          <div class="degree1 mt-3"><small class="fs-4 fw-bold fst-italic">Day </small>{{ weather.current.is_day }}
          </div>
          <div class="degree1 mt-3"><small class="fs-4 fw-bold fst-italic">Wind </small>{{ weather.current.wind_kph }}
            Km/hr
          </div>
          <div class="degree1 mt-3"><small class="fs-4 fw-bold fst-italic">Humidity </small>{{ weather.current.humidity
            }}
          </div>
          <div class="degree1 mt-3"><small class="fs-4 fw-bold fst-italic">Feel Like </small>{{
            weather.current.feelslike_c }}
          </div>
        </div>
      </div>
    </main>
    <marquee v-if="weather.location" class="marquee" direction="left" loop="">
            <div class="weather-info">
            <span class="location">{{ weather.location.name }}</span>
            <span class="temperature">{{ weather.current.temp_c }} °C</span>
            <span class="condition">{{ weather.current.temp_f }} °F</span>
            <span class="condition">{{ weather.current.text }}</span>
            <span class="condition">UV :{{ weather.current.uv }}</span>
            <span class="condition">Day :{{ weather.current.is_day }}</span>
            <span class="condition">Humidity :{{ weather.current.humidity }}</span>
            <span class="condition">Wind :{{ weather.current.wind_kph }}</span>
            <span class="condition">Last Update :{{ weather.current.last_updated }}</span>
            <span class="condition">Wind :{{ weather.current.wind_kph }}</span>
        </div> 
    </marquee>
  </div>
  </template>
  <script>
  import axios from 'axios';
  export default {
    name: 'App',
    data() {
      return {
        apiKey: 'e0bbcc47d81e421a92f111345241604',
        baseUrl: 'https://api.weatherapi.com/v1/current.json',
        // cityApiUrl: 'https://api.openweathermap.org/data/2.5/weather',
        city: '',
        defaultCity: 'Lucknow',
        weather: {},
      }
    },
    mounted() {
      this.city = this.defaultCity;
      this.fetchWeather();
    },
    methods: {
      getImageByTemp() {
    if (this.weather && this.weather.current && this.weather.current.temp_c) {
      let tempC=this.weather.current.temp_c
  
      if(tempC>=30){
        return 'bg-sunny'
      }
      else if(tempC>=20 && tempC<30){
        return 'bg-spring'
      }
      else if( tempC>=10 && tempC<20){
        return 'bg-rainy'
      }
      else if(tempC>=0 && tempC <10){
        return 'bg-snow'
      }
      else{
        return 'main'
      }
  
     
    }
    // Handle case when weather data is not available yet
    return '';
  },
  
      fetchWeather(e) {
        axios.get(`${this.baseUrl}?key=${this.apiKey}&q=${this.city}`)
          .then(response => {
            this.weather = response.data;
            //this.getImageByTemp();
            console.log(this.weather);
          })
          .catch(error => {
            console.error('Error fetching weather:', error);
          });
      },
    }
  }
  </script>