<template>
  <div class="container-fluid">
    <SearchBar @city-searched="fetchWeather" />
    <AreaDateTime :address="address" :formattedDate="formattedDate" :formattedTime="formattedTime"/>
    <Weather :temperature="temperature" />
    <WeatherIcon :icon="icon" :text="text" />
    <Forecast :humidity="humidity" :forecast="forecastData" />
    <HourlyForecast :forecast="forecastHourly" />

  </div>
</template>

<script>
import SearchBar from "./SearchBar.vue";
import AreaDateTime from "./AreaDateTime.vue";
import Weather from "./Weather.vue";
import WeatherIcon from "./WeatherIcon.vue";
import Forecast from "./Forecast.vue";
import HourlyForecast from "./HourlyForecast.vue";

import axios from "axios";

export default {
  components: { SearchBar, AreaDateTime, Weather, WeatherIcon, Forecast, HourlyForecast },
  data() {
    return {
      temperature: null,
      icon: null,
      text: null,
      address: null,
      date: null,
      humidity: null,
      forecastData: [],
      forecastHourly: [],
    };
  },
  computed: {
    formattedDate() {
      if (this.date) {
        const dateObj = new Date(this.date);
        const months = [
          "Jan",
          "Feb",
          "Mar",
          "Apr",
          "May",
          "Jun",
          "Jul",
          "Aug",
          "Sep",
          "Oct",
          "Nov",
          "Dec",
        ];
        const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

        const month = months[dateObj.getMonth()];
        const day = dateObj.getDate();
        const year = dateObj.getFullYear();
        const weekday = days[dateObj.getDay()];

        const formatted = `${month} ${day} ${year}, ${weekday}`;
        return formatted;
      }
      return "";
    },

    formattedTime() {
      if (this.date) {
        const dateString = this.date;
        const dateTime = new Date(dateString);

        const options = {
          hour12: true, 
          hour: "numeric",
          minute: "numeric", 
        };

        const time = dateTime.toLocaleTimeString("en-US", options);
        return time;
      }
    },
  },
  methods: {
    fetchWeather(city) {
      
      axios
        .get("https://weatherapi-com.p.rapidapi.com/forecast.json", {
          params: {
            q: city,
            days: 3,
          },
          headers: {
            "X-RapidAPI-Key":
            import.meta.env.VITE_WEATHER_API_KEY,
            "X-RapidAPI-Host": "weatherapi-com.p.rapidapi.com",
          },
        })
        .then((response) => {
          
          this.temperature = response.data.current.temp_c;
          this.icon = response.data.current.condition.icon;
          this.text = response.data.current.condition.text;
          this.humidity = response.data.current.humidity;
          this.forecastData = response.data.forecast.forecastday;
          this.forecastHourly = response.data.forecast.forecastday[0].hour;
          
          // console.log(this.forecastHourly);

          this.address =
            response.data.location.name + ", " + response.data.location.country;
          this.date = response.data.location.localtime;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    fetchUserLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;

            this.fetchUserCity(latitude, longitude);
          },
          (error) => {
            console.error(error.message);
          }
        );
      } else {
        console.error("Geolocation is not supported");
      }
    },
    fetchUserCity(latitude, longitude) {
      const apiKey = import.meta.env.VITE_OPENCAGE_API_KEY;

      axios
        .get(
          `https://api.opencagedata.com/geocode/v1/json?q=${latitude} ${longitude}&key=${apiKey}`
        )
        .then((response) => {
          const city = response.data.results[0].components.city;
          this.fetchWeather(city);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
  created() {
    this.fetchUserLocation();
  },
};
</script>

<style scoped>
</style>
