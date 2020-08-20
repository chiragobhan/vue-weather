<template>
  <div class="px-3 py-2 location-sidebar">
    <div :style="{marginTop:'60px'}">
      <b-form @submit="onSubmit" class="d-flex justify-content-between mt-3" novalidate>
        <b-input-group class="cityInputGroup align-items-center">
          <b-input-group-prepend>
            <b-icon-search class="lens"></b-icon-search>
          </b-input-group-prepend>
          <b-form-input
            autofocus
            id="cityInput"
            v-model="city"
            @change="validation = null"
            :state="validation"
            :placeholder="`search location`"
            required
            type="search"
          ></b-form-input>
        </b-input-group>
        <b-button class="submitBtn" type="submit" variant="primary" squared>Search</b-button>
      </b-form>
      <b-form-invalid-feedback :state="validation">City not found.</b-form-invalid-feedback>
      <div
        v-for="result in lastResults"
        :key="result.woeid"
        @click="$emit('citychange', result.woeid)"
        class="searchResult d-flex align-items-center justify-content-between mt-4"
      >
        <span>{{ result.title }}</span>
        <b-icon-chevron-right class="chevron"></b-icon-chevron-right>
      </div>
    </div>
  </div>
</template>

<script>
import { BIconChevronRight, BIconSearch } from "bootstrap-vue";
import axios from "axios";
export default {
  name: "LocationSidebar",
  components: {
    BIconChevronRight,
    BIconSearch,
  },
  data() {
    return {
      visible: false,
      weatherData: null,
      city: "",
      validation: null,
      recentSearch: [
        {
          latt_long: "19.076191,72.875877",
          location_type: "City",
          title: "Mumbai",
          woeid: 12586539,
        },
        {
          latt_long: "18.53611,73.85218",
          location_type: "City",
          title: "Pune",
          woeid: 2295412,
        },
        {
          latt_long: "28.643999,77.091003",
          location_type: "City",
          title: "New Delhi",
          woeid: 28743736,
        },
      ],
    };
  },
  computed: {
    lastResults() {
      const rev = [];
      rev.push(...this.recentSearch);
      rev.reverse();
      return rev.slice(0, 7);
    },
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault();
      if (!this.city) {
        alert("Please enter the search location");
      }
      axios
        .get(
          `https://cors-anywhere.herokuapp.com/https://www.metaweather.com/api/location/search/?query=${this.city}`
        )
        .then((response) => {
          if (response.data.length === 0) {
            this.validation = false;
          } else if (response.data.length === 1) {
            this.$emit("citychange", response.data[0].woeid);
            this.filterCity(response.data);
            this.recentSearch.push(...response.data);
            this.city = "";
          } else {
            this.filterCity(response.data);
            this.recentSearch.push(...response.data);
            this.city = "";
          }
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    filterCity(cities) {
      cities.forEach((city) => {
        this.recentSearch = this.recentSearch.filter(
          (response) => response.woeid !== city.woeid
        );
      });
    },
  },
  resetForm() {
    this.city = "";
    this.validation = null;
  },
};
</script>

<style lang="scss" scoped>
@import "./../assets/scss/locationSidebar.scss";
</style>