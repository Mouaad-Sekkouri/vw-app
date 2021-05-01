<template>
  <div
    class="wrapper"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16
        ? 'warm'
        : 'cold'
    "
  >
    <div id="app">
      <main>
        <div class="search-box">
          <input
            type="text"
            class="search-bar"
            placeholder="Type yor city..."
            v-model="query"
            @keypress.enter="getWeather"
          />
          <div @click="getWeather" class="search-btn">Search</div>
        </div>
        <transition name="height">
          <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
            <div class="location-box">
              <div class="location">
                {{ weather.name }}, {{ weather.sys.country }}
              </div>
              <div class="date">{{ dateBuilder() }}</div>
            </div>

            <div class="weather-box">
              <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
              <div class="weather">{{ weather.weather[0].main }}</div>
            </div>
          </div>
        </transition>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "252448664a1410ff81ac4fc7691d0121",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    getWeather() {
      fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setWeather);
    },
    setWeather(results) {
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
      let year = d.getFullYear();
      let month = months[d.getMonth()];
      let date = d.getDate();
      let day = days[d.getDay()];

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  letter-spacing: 0.5px;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  /* overflow: hidden; */
}

html,
body {
  overflow: hidden;
  transition: 0.2s !important;
}
* {
  transition: 0.2s !important;
}
.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 15px;
  margin: auto;
  min-height: 100vh;
}
.warm {
  background: url("./assets/warm.jpg") no-repeat bottom;
  background-size: cover;
}
.cold {
  background: url("./assets/cold.jpg") no-repeat bottom;
  background-size: cover;
}
#app {
  border-radius: 10px;
  background: #303f9fc7;
  box-shadow: 0 19px 38px rgba(0, 0, 0, 0.3), 0 15px 12px rgba(0, 0, 0, 0.22);
  max-width: calc(351px - 60px);
  width: 100%;
  height: calc(364px - 80px);
  overflow: hidden;

  padding: 40px 30px;
}

.search-box {
  display: flex;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  background: #3f51b5d7;

  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  border-radius: 10px;
  overflow: hidden;

  margin-bottom: 20px;
}
.search-box:hover {
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.25), 0 4px 5px rgba(0, 0, 0, 0.22);
}
.search-box input {
  width: 200px;
  padding: 10px;
  outline: 0;
  border: 0;
  background: transparent;
  border-radius: 10px;
  color: #ffffff;
  font-size: 15px;
}
.search-box input::placeholder {
  color: #dadada;
  font-size: 15px;
}
.search-box div {
  cursor: pointer;
  background: #448aff;
  padding: 10px;
  display: flex;
  align-items: center;
  color: #ffffff;
  font-weight: 600;
  line-height: 0;
}
.weather-wrap {
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  background: #3f51b5d7;
  padding: 20px 10px;
  border-radius: 10px;
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
}
.weather-wrap:hover {
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.25), 0 4px 5px rgba(0, 0, 0, 0.22);
}
.weather-wrap .location-box {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.weather-wrap .location-box .location {
  color: #ffffff;
  font-size: 25px;
  font-weight: 600;
}
.weather-wrap .location-box .date {
  color: #bdbdbd;
  font-size: 15px;
  font-weight: 400;
}

.weather-wrap .weather-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 30px;
}

.weather-wrap .weather-box .temp {
  color: #ffffff;
  font-size: 50px;
  font-weight: 900;
}
.weather-wrap .weather-box .weather {
  color: #bdbdbd;
  font-size: 25px;
  font-weight: 400;
}

.height-enter-active,
.height-leave-active {
  transition: 0.5s ease-in-out;
  transform: translateX(0);
  opacity: 1;
}
.height-enter,
.height-leave-to {
  transform: translateX(-400px);
  opacity: 0;
}
</style>