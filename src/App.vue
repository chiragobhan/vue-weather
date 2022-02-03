<template>
  <div id="app">
    <div class="loader" v-if="loading">
      <div class="lds-ring">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>
    <div v-else>
      <b-card no-body>
        <b-row no-gutters>
          <MainTemperatureComponent
            v-bind:weatherData="weatherData"
            :dateBuilder="dateBuilder"
            v-on:citychange="fetchData($event)"
          />
          <HighlightsComponent v-bind:weatherData="weatherData" />
        </b-row>
      </b-card>
    </div>
  </div>
</template>

<script>
import MainTemperatureComponent from "./components/MainTemperatureComponent";
import HighlightsComponent from "./components/HighlightsComponent";
import axios from "axios";

export default {
  name: "app",
  components: {
    MainTemperatureComponent,
    HighlightsComponent,
  },
  data() {
    return {
      weatherData: null,
      loading: false,
      currentCity: null,
    };
  },
  methods: {
    fetchData(woeid) {
      if (!woeid) {
        woeid = 12586539;
      } else if (woeid === this.currentCity) {
        return;
      }
      this.loading = true;
      axios
        .get(
          `https://cors-anywhere.herokuapp.com/https://www.metaweather.com/api/location/${woeid}/`
        )
        .then((response) => {
          this.weatherData = response.data;
          this.currentCity = response.data.woeid;
        })
        .catch((err) => console.log(err))
        .finally(() => (this.loading = false));
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sept",
        "Oct",
        "Nov",
        "Dec",
      ];
      let days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      return `${day}, ${date} ${month}`;
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<style lang="scss">
@import "node_modules/bootstrap/scss/bootstrap";
@import "node_modules/bootstrap-vue/src/index.scss";
@import "./assets/scss/main.scss";
@import "./assets/scss/layout.scss";
</style>
