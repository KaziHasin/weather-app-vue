<template>
  <div class="container-fluid">
    <SearchBar @city-searched="fetchWeather" />
    <AreaDateTime :address="address" :formattedDate="formattedDate" />
    <Weather :temperature="temperature" />
    <WeatherIcon :icon="icon" :text="text" />
    <Forecast :humidity="humidity" :forecast="forecastData"/>
  </div>
</template>

<script>
import SearchBar from "./SearchBar.vue";
import AreaDateTime from "./AreaDateTime.vue";
import Weather from "./Weather.vue";
import WeatherIcon from "./WeatherIcon.vue";
import Forecast from "./Forecast.vue";
import axios from "axios";

export default {
  components: { SearchBar, AreaDateTime, Weather, WeatherIcon, Forecast},
  data() {
    return {
      temperature: null,
      icon: null,
      text: null,
      address: null,
      date: null,
      humidity: null,
      forecastData: []
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
              "0a295a27f1msh10a3576bb9e7b92p12f60cjsneca0a590c6f0",
            "X-RapidAPI-Host": "weatherapi-com.p.rapidapi.com",
          },
        })
        .then((response) => {
          
          this.temperature = response.data.current.temp_c;
          this.icon = response.data.current.condition.icon;
          this.text = response.data.current.condition.text;
          this.humidity = response.data.current.humidity;
          this.forecastData = response.data.forecast.forecastday;
            console.log(this.forecastData[0]);
          
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
      const apiKey = "4019f8ee994242c5b70eec3a219c3f15";
        
      axios
        .get(`https://api.opencagedata.com/geocode/v1/json?q=${latitude} ${longitude}&key=${apiKey}`)
        .then((response) => {
         const city = response.data.results[0].components.city;
         this.fetchWeather(city);
        })
        .catch(function (error) {
          console.log(error);
        });
    }

  },
  created() {
  
    this.fetchUserLocation();
  },
};
</script>

<style scoped>
</style>
