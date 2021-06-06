<template>
  <b-col md="9" xs="12" sm="12" class="highlightsComponent">
    <div class="degree">
      <button type="button" class="btn degreeBtn mr-2 btn-secondary rounded-pill">°C</button>
    </div>
    <div class="alignCards">
      <b-card
        class="forthComing"
        v-for="(consolidated,index) in weatherData.consolidated_weather.slice(1, 6)"
        :key="consolidated.id"
      >
        <b-card-text>
          <h6 class="top">{{ index === 0 ? 'Tomorrow' : format_date(consolidated.applicable_date) }}</h6>
          <div class="imgHolder">
            <b-card-img-lazy
              :src="require(`./../assets/icons/${consolidated.weather_state_name}.png`)"
              alt="Weather Icon"
              class="rounded-0"
            ></b-card-img-lazy>
          </div>
          <p class="bottom">
            <span class="left">{{Math.round(consolidated.max_temp)}}°C</span>
            <span class="right">{{Math.round(consolidated.min_temp)}}°C</span>
          </p>
        </b-card-text>
      </b-card>
    </div>
    <div class="todaysHighlight">
      <h1 :style="{marginBottom:'25px'}">Today's Highlights</h1>

      <div class="col-md-12 highlightsCardHolder">
        <b-card class="col-md-5 col-sm-12 highlightsCard">
          <b-card-text>
            <h5 class="highlightsTop">Wind status</h5>
            <div class="windSpeedMiddle">
              <h1 class="mainStat">
                {{Math.round(weatherData.consolidated_weather[0].wind_speed)}}
                <span class="unit">mph</span>
              </h1>
            </div>
            <p class="highlightsBottom">
              <span
                class="badge windDirection mr-2 badge-secondary badge-pill"
                :style="compassDirection"
              >
                <svg
                  viewBox="0 0 16 16"
                  width="1em"
                  height="1em"
                  focusable="false"
                  role="img"
                  aria-label="cursor fill"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="currentColor"
                  class="bi-cursor-fill windArrow b-icon bi"
                >
                  <g>
                    <path
                      fill-rule="evenodd"
                      d="M14.082 2.182a.5.5 0 0 1 .103.557L8.528 15.467a.5.5 0 0 1-.917-.007L5.57 10.694.803 8.652a.5.5 0 0 1-.006-.916l12.728-5.657a.5.5 0 0 1 .556.103z"
                    />
                  </g>
                </svg>
              </span>
              {{weatherData.consolidated_weather[0].wind_direction_compass}}
            </p>
          </b-card-text>
        </b-card>

        <b-card class="col-md-6 col-sm-12 highlightsCard">
          <b-card-text>
            <h5 class="highlightsTop">Humidity</h5>
            <div class="middle">
              <h1 class="mainStat">
                {{Math.round(weatherData.consolidated_weather[0].humidity)}}
                <span class="unit">%</span>
              </h1>
            </div>
            <div class="highlightsBottom humidityProgress d-flex flex-column mr-auto ml-auto mt-2">
              <div class="barNotation d-flex justify-content-between">
                <span>0</span>
                <span class="middle">50</span>
                <span>100</span>
              </div>
              <div class="progress">
                <div
                  role="progressbar"
                  aria-valuemin="0"
                  aria-valuemax="100"
                  :aria-valuenow="Math.round(weatherData.consolidated_weather[0].humidity)"
                  class="progress-bar"
                  :style="displayProgress"
                ></div>
              </div>
              <span class="barNotation text-right">%</span>
            </div>
          </b-card-text>
        </b-card>
      </div>

      <div class="col-md-12 highlightsCardHolder">
        <b-card class="col-md-5 col-sm-12 highlightsCard">
          <b-card-text>
            <h5 class="highlightsTop">Visibility</h5>
            <div class="middle">
              <h1 class="mainStat">
                {{Math.round(weatherData.consolidated_weather[0].visibility)}}
                <span class="unit">miles</span>
              </h1>
            </div>
          </b-card-text>
        </b-card>

        <b-card class="col-md-6 col-sm-12 highlightsCard">
          <b-card-text>
            <h5 class="highlightsTop">Air Pressure</h5>
            <div class="middle">
              <h1 class="mainStat">
                {{Math.round(weatherData.consolidated_weather[0].air_pressure)}}
                <span class="unit">mb</span>
              </h1>
            </div>
          </b-card-text>
        </b-card>
      </div>

      <div :style="{marginTop: '40px',marginBottom: '40px',textAlign: 'center',opacity:'0.6'}">
        <p>
          <a target="_blank" style="color:white" href="https://github.com/chiragobhan">Chirag Obhan</a> @
          <a
            target="_blank"
            style="color:white"
            href="https://devchallenges.io/profile/eKaXBB0DeWix7Nsf84A5"
          >DevChallenges.io</a>
        </p>
      </div>
    </div>
  </b-col>
</template>

<script>
import moment from "moment";
export default {
  name: "HighlightsComponent",
  props: ["weatherData"],
  methods: {
    format_date(value) {
      if (value) {
        return moment(String(value)).format("ddd, DD MMM");
      }
    },
  },
  computed: {
    displayProgress() {
      return (
        "width: " + this.weatherData.consolidated_weather[0].humidity + "%;"
      );
    },
    compassDirection() {
      return (
        "transform: rotate(" +
        Math.round(this.weatherData.consolidated_weather[0].wind_direction) +
        "deg);"
      );
    },
  },
};
</script>

<style lang="scss" scoped>
@import "./../assets/scss/highlightsComponent.scss";
</style>
