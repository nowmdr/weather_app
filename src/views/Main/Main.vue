<template>
  <main class="main">
    <div class="main__container">
      <div v-if="loader" class="loader">Loading...</div>
      <div v-else class="card">
        <div class="card__header">
          <div class="card__location">
            <h1>{{ weather.location.name }}</h1>
            <p>{{ weather.location.country }}</p>
            <div>
              <p>{{ weather.current.condition.text }}</p>
              <p>Feel like{{ weather.current.feelslike_c }}°C</p>
            </div>
          </div>
          <div class="card__condition">
            <p class="temperature">{{ weather.current.temp_c }}°C</p>
            <div class="card__image">
              <img
                :src="`https:${weather.current.condition.icon}`"
                alt="weather-description"
              />
            </div>
          </div>
        </div>
        <div class="card__body body">
          <!-- <div class="body__header">
            <p>More info</p><button class="card__button plus">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="ionicon"
                viewBox="0 0 512 512"
              >
                <title>Add</title>
                <path
                  fill="none"
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="32"
                  d="M256 112v288M400 256H112"
                />
              </svg>
            </button>
          </div> -->
          <p class="body__element time">
            {{ weather.location.localtime.split(" ")[1] }}
          </p>
          <p class="body__element cloud">{{ weather.current.cloud }} %</p>
          <p class="body__element wind">{{ weather.current.wind_mph }} m/h</p>
          <p class="body__element humidity">{{ weather.current.humidity }} %</p>
        </div>
        <form @submit.prevent="fetchWeather()" class="card__form">
          <input
            v-model="city"
            placeholder="Type your city"
            class="card__input"
            type="text"
          />
        </form>
      </div>
    </div>
  </main>
</template>
<script>
import "../Main/Main.scss";
import axios from "axios";
export default {
  name: "Main",
  components: {},
  data: () => ({
    city: "London",
    weather: {},
    image: require("@/assets/cloud.jpg"),
    loader: true,
  }),
  mounted() {
    this.fetchWeather();
  },
  methods: {
    async fetchWeather() {
      const key = process.env.VUE_APP_WEATHER;
      try {
        const data = await (
          await axios.get(
            `https://api.weatherapi.com/v1/current.json?key=${key}&q=${this.city}&aqi=no`
          )
        ).data;
        console.log(data);
        this.city = "";
        this.weather = data;
        console.log(
          "🚀 ~ file: Main.vue ~ line 36 ~ fetchWeather ~ this.weather",
          this.weather
        );
        this.loader = false;
      } catch (error) {
        console.log(
          "🚀 ~ file: Main.vue ~ line 27 ~ fetchWeather ~ error",
          error
        );
      }
    },
  },
};
</script>