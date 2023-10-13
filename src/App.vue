<script>
import axios from 'axios';
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed : {
    cityName() {
      return '"' + this.city + '"'
    },
    showTemp() {

      return 'Temperature ' + this.info.main.temp
    },
    showFeelsLike() {
      return 'Feels like ' + this.info.main.feels_like
    },
    showMinTemp() {
      return 'Min temperature ' + this.info.main.temp_min
    },
    showMaxTemp() {
      return 'Max temperature ' + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'need the name of more than one character';
        return false
      }

      this.error = ''

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
          .then(response => (this.info = response.data))

    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Weather Application </h1>
    <p>find out the weather {{city =="" ? "in your city" : "in " + cityName}} </p>
    <input type="text"
           v-model="city"
           placeholder="Enter city">
    <button v-if="city !=='' "
            @:click="getWeather()"
    >get the weather</button>
    <button v-else disabled>enter the name of the city</button>
    <p class="error">{{ error }}</p>
    <div v-if="info !== null">
      <p >{{ showTemp }}°C</p>
      <p >{{ showFeelsLike }}°C</p>
      <p >{{ showMinTemp }}°C</p>
      <p >{{ showMaxTemp }}°C</p>
    </div>

  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  padding: 20px;
  text-align: center;
  color: #fff;
}

.wrapper h1{
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  outline: none;
  padding: 5px 8px;
  transition: all .2s;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms  ease;
}

.wrapper button:disabled {
  background: #746027;
  cursor: not-allowed;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
