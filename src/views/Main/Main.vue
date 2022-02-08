<template>
  <main class="main">
    <div class="main__container">
      <div v-if="loader" class="loader">Loading...</div>
      <div v-else class="card">
        <div class="card__header">
          <h1>{{ weather.location.name }}</h1>
          <p>{{ weather.location.country }}</p>
          <p>Local time: {{ weather.location.localtime.split(" ")[1] }}</p>
        </div>
        <div class="card__body">
          <p>{{ weather.current.temp_c }}°C</p>
          <p>Feel like{{ weather.current.feelslike_c }}°C</p>
          <p>{{ weather.current.condition.text }}</p>
          <div class="card__image">
            <img
              :src="`https:${weather.current.condition.icon}`"
              alt="weather-description"
            />
          </div>
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