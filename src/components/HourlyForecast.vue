<template>
    <h4 class="heading">Hourly</h4>
    <div class="box rounded-lg px-2 flex items-center">

      <div class="forecast-box flex justify-between text-lg grow flex-nowrap">
        <div v-for="(day, index) in forecast" 
        :key="day.date" 
        :class="['forecast', 'flex', 'flex-col', 'justify-center', 'items-center', index === 0 ? 'ml-0' : 'mx-4']">
          <div class="date mb-2 text-sm">{{formattedDate(day.time)}}</div>
          <div class="icon"><img :src="day.condition.icon" alt="" class="w-12 h-12"></div>
          <div class="weather text-center text-sm">{{ day.temp_c }} <sup>o</sup></div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { Icon } from "@iconify/vue";
  
  const formattedDate = (time) => {
    const dateString = time;
        const dateTime = new Date(dateString);

        const options = {
          hour12: true, 
          hour: "numeric",
          minute: "numeric", 
        };

        const FormatTime = dateTime.toLocaleTimeString("en-US", options);
        return FormatTime;
   
  };
  </script>
  <script>
     export default {
      props : ['forecast'],
    
    }
   
    
  </script>
  
  <style scoped>
  .heading {
    width: 90%;
    margin:0 auto;
  }
  .box {
    position: relative;
    width: 90%;
    min-height: 200px;
    box-shadow: 0 15px 35px rgba(0, 0, 0, 0.25),
                0 -15px 35px rgba(0, 0, 0, 0.25);
    margin: 5px auto 50px;
  }
  
  .forecast-box {
  overflow-x: auto;  
  white-space: nowrap; 
}

.forecast-box::-webkit-scrollbar {
  width: 0;
}

.forecast-box {
  scrollbar-width: none;
}
  </style>