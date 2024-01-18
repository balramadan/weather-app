<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>

      <div class="weather-wrap" v-if="(typeof weather.main != 'undefined')">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="weather-temp">
            <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
          <div class="feelslike">
            Feels Like {{ Math.round(weather.main.feels_like) }}°C
          </div>
          <div class="wind">
            <div class="deg">
              <div class="">Degree</div>
              <div class="">{{ weather.wind.deg }}°</div>
            </div>
            <div class="speed">
              <div class="">Speed</div>
              <div class="">{{ weather.wind.speed }} m/s</div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "41ee5ee739826f66034e9b65de96bda1",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "Juny",
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

      return `${day}, ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  top: 0;
  left: 0;
  box-sizing: border-box;
}

body {
  font-family: "monserrat", sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
}

.weather-temp {
  padding: 10px 25px;
  color: #fff;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-temp .temp {
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-temp .weather {
  font-size: 48px;
}

.feelslike {
  color: #fff;
  font-size: 24px;
  background-color: rgba(255, 255, 255, 0.25);
  padding: 10px 25px;
  margin-bottom: 30px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.wind {
  display: flex;
  flex-direction: row;
  color: #fff;
  background-color: rgba(255, 255, 255, 0.25);
  padding: 10px 25px;
  justify-content: space-between;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.wind .deg {
  margin-right: 20px;
}

@media only screen and (max-width: 600px) {
  .temp {
    font-size: 82px;
    margin: 15px 0;
  }

  .weather {
    font-size: 34px;
  }
}
</style>
