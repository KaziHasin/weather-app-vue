<template>
  <div class="area-box flex flex-col justify-center my-5 mt-10">
    <div class="area text-center">
      <span
        class="text-white font-medium tracking-wide font-mono text-xl md:text-4xl"
        >{{ address }}</span
      >
    </div>

    <div class="date_time mt-2">
      <span class="text-white font-light font-mono text-xl">
        {{ formattedDate }}
      </span>
    </div>
    <div class="time">
      <span>
        {{ formattedTime }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["address", "formattedDate", "formattedTime"],
};
</script>

<style scoped>
.area-box {
  align-items: center;
}
</style>