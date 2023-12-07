<template>
  <div class="app">
    <div class="header container h-100 p-4">
      <h1 class="mb-3">Weather App</h1>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input
            type="text"
            class="input form-control"
            placeholder=" Enter a City"
            v-model="city"
            @keyup.enter="searchWeather"
          />
        </div>
        <button class="btn-search btn btn-primary" @click="searchWeather">
          Search <i class="fas fa-search"></i>
        </button>
      </div>
    </div>
  </div>
  <br />
  <Weather v-if="showWeather" :city="city"></Weather>
</template>

<script>
import Weather from "./components/Weather.vue";
export default {
  name: "App",
  components: {
    Weather,
  },
  data() {
    return {
      city: "",
      showWeather: false,
    };
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    },
  },
};
</script>

<style>
@import "~@fortawesome/fontawesome-free/css/all.css";
@import "./assets/css/App.css";
</style>
