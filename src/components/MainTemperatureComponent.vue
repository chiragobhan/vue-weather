<template>
  <b-col md="3" xs="12" sm="12" class="mainTemperatureComponent">
    <b-button class="search-location" v-b-toggle.location>Search for Places</b-button>
    <!-- <span class="circle"></span> -->
    <!-- <b-img-lazy class="location-icon" :src="require('./../assets/icons/location.png')" fluid></b-img-lazy> -->
    <b-sidebar width="375px" no-header id="location" shadow v-slot:default="{ hide }">
      <button @click="hide" type="button" aria-label="Close" class="close text-light">
        <svg
          viewBox="0 0 16 16"
          width="1.5em"
          height="2em"
          focusable="false"
          role="img"
          aria-label="x"
          xmlns="http://www.w3.org/2000/svg"
          fill="currentColor"
          class="bi-x b-icon bi"
        >
          <g>
            <path
              fill-rule="evenodd"
              d="M11.854 4.146a.5.5 0 0 1 0 .708l-7 7a.5.5 0 0 1-.708-.708l7-7a.5.5 0 0 1 .708 0z"
            />
            <path
              fill-rule="evenodd"
              d="M4.146 4.146a.5.5 0 0 0 0 .708l7 7a.5.5 0 0 0 .708-.708l-7-7a.5.5 0 0 0-.708 0z"
            />
          </g>
        </svg>
      </button>
      <LocationSidebar v-on:citychange="$emit('citychange',$event)" />
    </b-sidebar>
    <b-card-img-lazy
      :src="require('./../assets/icons/Cloud-background.png')"
      alt="Cloud Background"
      class="rounded-0 cloudBg"
    ></b-card-img-lazy>
    <b-card-img-lazy
      center
      :src="require(`./../assets/icons/${weatherData.consolidated_weather[0].weather_state_name}.png`)"
      alt="Weather Icon"
      class="rounded-0 weatherIcon"
    ></b-card-img-lazy>
    <div
      class="temperatureHolder"
      v-if="typeof weatherData.consolidated_weather[0].the_temp != 'undefined'"
    >
      <h1 class="temperature">
        {{Math.round(weatherData.consolidated_weather[0].the_temp)}}<span class="degree">°C</span>
      </h1>
    </div>
    <div class="weatherStatus">
      <h2 class="weatherStatusText">{{weatherData.consolidated_weather[0].weather_state_name}}</h2>
    </div>
    <div class="date">
      <p :style="{fontSize:'18px'}">Today &bull; {{dateBuilder()}}</p>
    </div>
    <div class="city">
      <p :style="{fontSize:'18px'}">{{weatherData.title}}</p>
    </div>
  </b-col>
</template>

<script>
import LocationSidebar from "./LocationSidebar";
export default {
  name: "MainTemperatureComponent",
  components: {
    LocationSidebar,
  },
  props: {
    weatherData: { type: Object },
    dateBuilder: { type: Function },
  },
  mounted() {
    this.dateBuilder();
  },
};
</script>

<style lang="scss" scoped>
@import "./../assets/scss/mainTemperatureComponent.scss";
</style>
