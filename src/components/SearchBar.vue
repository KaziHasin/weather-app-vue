<template>
  <div class="w-full flex justify-center relative">
    <input
      type="text"
      class="rounded-full shadow-lg p-4 mt-12 border text-white text-xl border-gray-500 outline-0"
      id="search"
      placeholder="Search city..."
      v-model="city"
     
    />
    <Icon icon="ic:baseline-search" class="absolute top-16 right-12 text-4xl cursor-pointer search-icon" @click="searchCity"/>
  </div>
</template>

<script setup>
import { Icon } from '@iconify/vue';
</script>
<script>
export default {
 
  data() {
    return {
      city: "",
    };
  },
  methods: {
    searchCity() {
      this.$emit("city-searched", this.city);
    },
  },
};
</script>

<style scoped>
#search {
  background: transparent;

  width: 90%;
}
.search-icon {
  opacity: .6;
}
</style>
