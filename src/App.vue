<template
  ><div
    id="app"
    :class="
      typeof state.weather.main != 'undefined' &&
      Math.round(state.weather.main.temp) > 16
        ? 'warm'
        : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="state.query"
          @keypress="fetchWeather"
        />
      </div>
      <div
        class="weather-wrap"
        v-if="typeof state.weather.main !== 'undefined'"
      >
        <div class="location-box">
          <div class="location">
            {{ state.weather.name }}, {{ state.weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ state.weather.main.temp }}℃</div>
          <div class="weather">{{ state.weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { reactive } from "vue"

export default {
  name: "App",
  setup() {
    const state = reactive({
      api_key: "8039d224abede960468e5e0c9e9b5477",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    })

    const fetchWeather = (e) => {
      if (e.key == "Enter") {
        let fetchUrl = `${state.url_base}weather?q=${state.query}&units=metric&APPID=${state.api_key}`
        fetch(fetchUrl)
          .then((res) => {
            console.log(res)
            return res.json()
          })
          .then((results) => {
            return setResult(results)
          })
      }
    }

    const setResult = (results) => {
      state.weather = results
    }

    const dateBuilder = () => {
      let d = new Date()
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
      ]
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ]
      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()
      return `${day} ${date} ${month} ${year}`
    }
    return {
      state,
      fetchWeather,
      setResult,
      dateBuilder,
    }
  },
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
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
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
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
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
