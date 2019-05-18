<template>
  <div id="weather-app">
    <hgroup>
          <h1>Weather App</h1>
          <h2>With Vue and Open Weather</h2>
    </hgroup>

    <label for="location">
      Weather at:
      <input type="text" name="location" @keyup.enter="getWeather" placeholder="location" v-model="location">
      </label>
      <button type="button" @click="getWeather" >Get Weather</button>
      <div class="weather-status">
        <img :src="iconUrl" v-if="currentWeather"> <div v-if="currentWeather">Weather: <span>{{weatherStatus}}</span></div>
      </div>
      <div class="error-message" v-if="errorMessage">
        {{errorMessage}}
      </div>
      <div class="credits">
        <h2>Credits</h2>
        <p>Author: Jason McKee</p>
        <p> <a href="https://www.theodinproject.com/lessons/weather-app">An Odin Project Assignment</a></p>
        <p>Weather data and icons from <a href="https://openweathermap.org/"> Open Weather Map</a> </p>


      </div>
      <div class="loader" v-if="loading">
        <div class="line line1">
        </div>
        <div class="line line2">
        </div>
        <div class="line line3">
        </div>
        <div class="line line4">
        </div>
        <div class="line line5">
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'weather-app',
  methods: {
    setWeatherIcon() {
      this.iconUrl = `http://openweathermap.org/img/w/${this.currentWeather.weather[0].icon}.png`
    },
    setWeatherStatus() {
      this.weatherStatus = this.currentWeather.weather[0].description
    },
    getWeather: async function() {
      try {

        this.currentWeather = ''
        this.errorMessage = ''
        this.loading = true
        const weatherResponse = await
        fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&APPID=1916709f8ab517d25cbb702d6b210a9e`)
        console.log(weatherResponse)
        if (weatherResponse.ok) {
          this.currentWeather = await weatherResponse.json()
          this.setWeatherIcon()
          this.setWeatherStatus()
        } else if (weatherResponse.statusText === 'Not Found') {
          this.errorMessage = 'Location not found'
        } else {
          this.errorMessage = `Error retrieving data.
          ${weatherResponse.statusText}  Please try again in a few minutes.`
        }
      } catch(err) {
        this.errorMessage =`  ${err}
          Trouble shooting: Please check your internet connection and try again in a few minutes.`
      }
      this.loading = false
    }
  },
  data () {
    return {
      location: 'London',
      iconUrl: '#',
      currentWeather: '',
      weatherStatus: '',
      errorMessage: '',
      loading: false
    }
  },
  mounted( ){
    this.getWeather()
  }
}
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css?family=Bitter:400,400i,700|Josefin+Sans:300,300i,400,400i,600,600i,700,700i|Noto+Sans+JP:100,300,400,500,700,900&display=swap');
  $light-yellow: #FFFFF0;
  $go-green: #00C000;
  @mixin primary-heading {
    font-family: 'Noto Sans JP', Arial, sans-serif;
    font-weight: 900;
    font-style: normal;
    font-size: 1.6rem;
    margin-top: 12px;
    margin-bottom: 12px;

  }
  @mixin subheading {
    font-family: 'Josefin Sans', 'MS Comic Sans', Arial, sans-serif;
    font-weight: bold;
    font-size: 1.25rem;
    font-style: italic;
  }
  @mixin plain-text{
    font-size: 1.2rem;
    font-family: 'Bitter', serif;
  }
  @mixin button-text {
    font-family: 'Noto Sans JP', Arial, sans-serif;
    font-weight: 500;
  }
  #weather-app {
    background-color: #EEE;
    *{
      box-sizing: border-box;
    }
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 25px;
    display: grid;
    grid-template-columns: minmax(300px, 450px);
    grid-auto-flow: row;
    grid-gap:20px;
    justify-content: center;
    align-content: start;
    @include plain-text;
    position: relative;

    input {
    @include plain-text;
    font-size: 90%;
    border-radius: 5px;
    background-color: $light-yellow;
    width: 100%;
    margin-right: 3px;
    }
    button {
    @include button-text;
    font-size: 1.4rem;
    height: 2.6rem;
    border-radius: 1.3rem;
    display: grid;
    align-items: center;
    background-color: $go-green;
    color: white;
    width: 100%;
    margin: 0;
    }
    h1 {
      @include primary-heading;
      text-align: center;
    }
    hgroup h2 {
      text-align: center;
      @include subheading;
    }
    label {
      display: grid;
      grid-template-columns: auto 1fr;
      align-items: center;
      grid-gap: 7px;
    }
    .weather-status {
      display: grid;
      grid-template-columns: auto  1fr;
      align-items: center;
      grid-gap: 4px;
      span {
        font-style: italic;
        font-weight: light;
      }
      div {
        display: grid;
        grid-template-columns: auto  1fr;
        align-items: baseline;
        grid-gap:4px;
      }
    }
    .error-message {
      color: red;
    }
    .credits {
      margin-top: 75px;
      font-size: 0.75rem;
      font-family: Arial, sans-serif;
      font-style: italic;
      color: grey;
      a {
        color:grey;
      }
      h2 {
        font-weight: bold;
      }
    }
  }
  @keyframes rotateFrame {
    0% {
      transform: rotateZ(0deg);
      background: linear-gradient( to right,
      rgba(255,0,0,0.3),
      transparent);
    }

    54.99% {
      transform: rotateZ(360deg);
      background: linear-gradient( to right,
      rgba(255,0,0,0.3),
      transparent);
    }
    55% {
      transform: rotateZ(0deg);
      background: linear-gradient( to right,
      rgba(255,0,0,0.3),
      transparent);

    }
    75% {
      background: transparent;

    }
    100% {
      transform: rotateZ(0deg);
      background: transparent;
    }

  }
  .loader {
    position: relative;
    height: 400px;
    width: 400px;
    position: absolute;
    left: calc(50% - 130px);
    top: 150px;
    .line {
      background: transparent;
      height: 25px;
      width: 120px;
      border-radius: 20px;
      transform-origin: 140px 50%;
      //animation: rotateFrame 600 linear 0 infinite normal;
      animation-name: rotateFrame;
      animation-duration: 2500ms;
      animation-timing-function: linear;
      animation-delay: 0;
      animation-iteration-count: infinite;
      animation-direction: normal;
      &.line1 {
        position: absolute;

      }
      &.line2 {
        position: absolute;
        animation-delay: 250ms;
      }
      &.line3 {
        position: absolute;
        animation-delay: 500ms;
      }
      &.line4 {
        position: absolute;
        animation-delay: 750ms;
      }
      &.line5 {
        position: absolute;
        animation-delay: 1000ms;
      }
    }

  }
</style>
