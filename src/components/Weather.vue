<template>
  <div class="container p-0">
    <div
      v-if="cityname == 'notFound'"
      class="d-flex justify-content-center text-light"
    >
      <h1>NotFound</h1>
    </div>
    <div class="d-flex row" v-else>
      <div class="card main-div col-sm-12 col-md-6">
        <div class="p-3">
          <h2 class="mb-1 day">Today</h2>
          <p class="text-light date mb-0">{{ date }}</p>
          <small>{{ time }}</small>
          <h2 class="place">
            <i class="fa fa-location"
              >{{ name }} <small>{{ country }}</small></i
            >
          </h2>
          <div class="temp">
            <h1 class="weather-temp">{{ temperature }}&deg;</h1>
            <h2 class="text-light">
              {{ description }}
              <img :src="iconUrl" alt="icon" />
            </h2>
          </div>
        </div>
      </div>

      <div class="card card-2 col-sm-12 col-md-6">
        <table class="m-4">
          <tbody>
            <tr v-if="sea_level > 0">
              <th>Sea level</th>
              <td>{{ sea_level }}</td>
            </tr>
            <tr>
              <th>Humidity</th>
              <td>{{ humidity }}</td>
            </tr>
            <tr>
              <th>Wind</th>
              <td>{{ wind }}</td>
            </tr>
          </tbody>
        </table>
        <days-weather :cityName="cityname" />

        <div id="div_Form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input
              type="button"
              value="Change Location"
              class="btn change-btn btn-primary"
              @click="changeLocation"
            />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import DaysWeather from "./DaysWeather.vue";
export default {
  name: "myWeather",
  components: {
    DaysWeather,
  },
  props: {
    city: String,
  },
  data() {
    return {
      cityname: this.city,
      temperature: null,
      description: null,
      iconUrl: null,
      date: null,
      time: null,
      name: "",
      sea_level: null,
      wind: null,
      humidity: null,
      country: null,

      monthName: [
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
      ],
    };
  },
  methods: {
    changeLocation() {
      window.location.reload();
    },
  },
  async created() {
    await axios
      .get(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8331cdef4f10633c84fd856ce65588b0`
      )
      .then((response) => {
        const WeatherData = response.data;
        console.log(response.data);
        this.temperature = Math.round(WeatherData.main.temp);
        this.description = WeatherData.weather[0].description;
        this.name = WeatherData.name;
        this.iconUrl = `https://api.openweathermap.org/img/w/${WeatherData.weather[0].icon}.png`;
        const d = new Date();
        this.time = d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds();
        this.date =
          d.getDate() +
          "-" +
          this.monthName[d.getMonth()] +
          "-" +
          d.getFullYear();
        this.wind = WeatherData.wind.speed;
        this.sea_level = WeatherData.main.sea_level;
        this.country = WeatherData.sys.country;
        this.humidity = WeatherData.main.humidity;
      })
      .catch((error) => {
        console.error("Error Fetching weather data:", error);
        this.cityname = "notFound";
      });
  },
};
</script>

<style scoped>
@import "../assets/css/Weather.css";
</style>
