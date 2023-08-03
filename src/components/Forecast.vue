<template>
  <h4 class="heading">Daily</h4>
  <div class="box flex justify-between rounded-lg px-2 items-center md:px-6">
    <div class="humidity flex items-center text-xl border-r-2 border-white">
      <Icon icon="carbon:humidity" class="text-xl md:text-2xl" />
      <div class="px-2 pr-1 md:pr-12">
        <span class="text-sm md:text-lg">Humidity</span>
        <br />
        <span class="text-sm md:text-lg">{{ humidity }}%</span>
      </div>
    </div>

    <div class="forecast-box flex justify-between text-xl">
      <div
        v-for="day in forecast"
        :key="day.date"
        class="forecast flex flex-col justify-center items-center"
      >
        <div class="date mb-2 text-sm md:text-lg">
          {{ getDayOfWeek(day.date) }}
        </div>
        <div class="icon">
          <img
            :src="day.day.condition.icon"
            alt=""
            class="w-10 h-10 md:w-16 md:h-16"
          />
        </div>
        <div class="weather text-center text-sm">
          {{ day.day.avgtemp_c }} <sup>o</sup>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { Icon } from "@iconify/vue";
const getDayOfWeek = (date) => {
  const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
  const dayIndex = new Date(date).getDay();
  return days[dayIndex];
};
</script>
<script>
export default {
  props: ["humidity", "forecast"],
};
</script>

<style scoped>
.heading {
  width: 90%;
  margin: 50px auto 0;
}
.box {
  position: relative;
  width: 90%;
  min-height: 200px;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.25), 0 -15px 35px rgba(0, 0, 0, 0.25);
  margin: 5px auto 50px;
}
.forecast-box {
  flex: 0.7;
}
.humidity {
  flex: 0.2;
}

@media (max-width: 768px) {
  .forecast-box {
    flex: 0.8;
  }
  .humidity {
    flex: 0.1;
  }
}
</style>