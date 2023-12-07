<template>
  <div class="days-tab text-center">
    <div v-if="loading" class="loading">Loading....</div>
    <ul v-else class="p-0">
      <li v-for="day in forecast" :key="day.data" class="li-active">
        <div class="py-3"><img :src="day.iconUrl" /></div>
        <div class="py-3">{{ getDayName(day.data._d) }}</div>
        <div class="py-3">{{ day.temperature }}&deg;</div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  name: "days-weather",
  props: {
    cityName: String,
  },
  data() {
    return {
      forecast: [],
      loading: true,
      iconUrl: null,
    };
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    async fetchWeatherData() {
      console.log(this.cityName);
      const apiKey = "8331cdef4f10633c84fd856ce65588b0";
      const city = this.cityName;
      const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;
      await axios
        .get(apiUrl)
        .then((Response) => {
          const ForecastData = Response.data.list;
          const filteredData = ForecastData.map((item) => {
            return {
              data: moment(item.dt_txt.split(" ")[0]),
              temperature: Math.round(item.main.temp),
              description: item.weather[0].description,
              iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
            };
          })
            .reduce((acc, item) => {
              if (
                !acc.some((day) =>
                  // console.log(day, "day");
                  day.data.isSame(item.data, "day")
                )
              ) {
                acc.push(item);
              }
              return acc;
            }, [])
            .slice(1, 5);
          // console.log(filteredData, "working");
          this.forecast = filteredData;
          this.loading = false;
        })
        .catch((error) => {
          console.error("Error Fetching weather data:", error);
          this.loading = false;
        });
    },
    getDayName(date) {
      // console.log(date);
      return moment(date).format("ddd");
    },
  },
};
</script>

<style scoped>
@import "../assets/css/DaysWeather.css";
</style>
