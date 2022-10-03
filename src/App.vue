<template>
  <div
    class="app"
    :style="`background-image: url(` + require(`./assets/bg${idx}.jpg`) + ')'"
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress.enter="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-show="weather_country != ''">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather_country }}</div>
          <div class="data">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ weather_temp }}°c</div>
          <div class="weather">{{ weather_main }}</div>
          <div class="wind">wind speed {{ weather_wind }} ms</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "9d3710464c2baa7c63035b66f699e7bf",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      weather_country: "",
      weather_temp: "",
      weather_main: "",
      weather_wind: "",
      idx: 0,
    };
  },

  beforeMount() {
    this.backgroundChange();
  },

  methods: {
    backgroundChange() {
      let self = this;
      setInterval(function () {
        if (self.idx > 8) {
          self.idx = 0;
        }
        self.idx += 1;
      }, 5000);
    },
    fetchWeather() {
      if (this.query) {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      } else {
        this.weather_country = this.weather_temp = this.weather_main = "";
      }
    },
    setResults(results) {
      console.log(results);
      this.weather = results;
      this.weather_country = this.weather.sys.country;
      this.weather_temp = Math.round(this.weather.main.temp);
      this.weather_main = this.weather.weather[0].main;
      this.weather_wind = Math.round(this.weather.wind.speed);
      let d = new Date();
      console.log(d);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
.app {
  background-size: cover;
  background-position: top;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 25px;
  /* background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  ); */
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px;
  transition: 0.4s;
}

.search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0;
}

.location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.data {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(66, 70, 71, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.wind {
  color: #fff;
  font-size: 30px;
  font-weight: 500;
}
</style>
