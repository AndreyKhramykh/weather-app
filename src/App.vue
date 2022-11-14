<template>
  <div class="wrapper">
      <h1>Weather app</h1>
      <p>The weather in {{city == '' ? 'your city' : cityName }}</p>
      <div class="input-field">
        <input class="content-box__input" v-on:keyup.enter="getWeather()" v-model="city" placeholder="Choose your city" type="text">
        <button class="content-box__button" @click="getWeather()">Start</button>
      </div>
      <p v-if="error != ''" class="error"> {{ error }}</p>
      <div v-if="info != null" class="info">
        <div class="mb10px animationItem delay2s"> {{ showWeatherDescription }} </div>
        <div class="mb10px animationItem delay3s"> {{ showTemperature }} </div>
        <div class="mb10px animationItem delay4s"> {{ showTemperatureFeelsLike }} </div>
        <img class="info__img" :src="require(`@/assets/weathersPicture/${this.showTheIcon}`)" alt="">
      </div>
  </div>
  <div class="footer">All API and images was token from
    <a target="_blank" href="https://openweathermap.org/">openweathermap</a>
  </div>
</template>

<script>

  import axios from 'axios'

  export default {
    data() {
      return {
        city: '',
        error: '',
        info: null
      }
    },

    computed: {
      cityName() {
        return '«' + this.city + '»'
      },
      showWeatherDescription() {
        return "Weather: " + this.info.data.weather[0].description
      },
      showTemperature() {
        return "Temperature: " + JSON.stringify(this.info.data.main.temp) + '°C'
      },
      showTemperatureFeelsLike() {
        return "Temperature feels like: " + JSON.stringify(this.info.data.main.feels_like) + '°C'
      },
      showTheIcon() {

        return this.info.data.weather[0].icon + '.png'
      },
      getTheWeatherIcon() {
        return `@/assets/weathersPicture/${this.showTheIcon}`
      },
      showData() {
        return 'data' + JSON.stringify(this.info.data)
      }
    },

    methods: {
      getWeather() {

        this.info = null

        if(this.city == '') {
          this.error = 'Enter the city!';
          return false

        }
        
        if(this.city.trim().length < 2) {
          this.error = 'Wrong city name!';
          return false
        }

        this.error = '';


        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3e300fc82afadfdf01e287566db6b07e`)
            .then(res => {this.info = res})
            .catch( () => {
              this.error = 'This city does not exist'
            })
      }
    }
  }

</script>

<style lang="scss">

@font-face {
  font-family: Ubuntu;
  src: url(@/assets/fonts/Ubuntu-Regular.ttf);
}

* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}

body {
    width: 100%;
    height: 100vh;
    font-family: Ubuntu;
    color: white;
    text-shadow: 2px 2px 2px black;
}

a {
  text-decoration: none;
  color: #423f57;
  text-shadow: none;
}
  #app {
    width: 100%;
    height: 100vh;
    background: rgb(131,58,180);
    background: linear-gradient(90deg, rgba(131,58,180,1) 0%, rgba(253,29,29,1) 50%, rgba(252,176,69,1) 100%);
    display: grid;
    place-items: center;
  }

  .wrapper {
    padding: 40px;
    background-color: #595570;
    border-radius: 10px;
    box-shadow: 0px 0px 20px 10px rgba(black, .6);
    min-width: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;

    & h1 {
      margin: 20px;
    }

    & p {
      margin: 10px;
    }
  }

  .content-box {

    &__input {
      outline: none;
      border: none;
      background-color: #615d7d;
      box-shadow: 0px 0px 8px 4px rgba($color: #000000, $alpha: .2);
      color: white;
      width: 50%;
      padding: 6px;
      border-radius: 4px;

      &::placeholder {
        color: rgb(255, 255, 255, 0.3);
      }
    }

    &__button {
      background-color: rgb(46, 43, 59);
      border-radius: 4px;
      font-weight: bold;
      padding: 6px;
      box-shadow: 0px 0px 10px 5px rgba($color: #000000, $alpha: .2);
      border: none;
      cursor: pointer;
      color: white;
      width: 20%;

      &:hover {
        background-color: rgb(46, 43, 59, .8);
        box-shadow: 0px 0px 5px 2px rgba($color: #000000, $alpha: .2);
      }
    }
  }

  .input-field {
    margin: 10px;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    width: 100%;
  }

  .info {
    display: flex;
    flex-direction: column;
    width: 100%;
    position: relative;
    margin-top: 10px;
    overflow: hidden;

    &__img {
      position: absolute;
      top: 0;
      right: 0;
      animation: appearWeatherImage 6s ease-out 1;

    }


  }

  .footer {
    text-align: center;
  }

  .error {
    color: red;
    text-shadow: none;
  }

  .mb10px {
    margin-bottom: 10px;
  }

  .animationItem {
    animation: appearWeatherItem 2s ease-out 1;
  }

  .delay2s {
    animation: appearWeatherItem 2s;
  }
  .delay3s {
    animation: appearWeatherItem 3s;
  }
  .delay4s {
    animation: appearWeatherItem 5s;
  }

  @keyframes appearWeatherItem {
    0% {
      transform: translateX(-200%);
    }
    100% {
      transform: translateX(0%);
    }
  }
  @keyframes appearWeatherImage {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }

  @media screen and (max-width: 768px) {
    .info__img {
      display: none;
    }
  }

</style>


