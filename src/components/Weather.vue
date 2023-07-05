<template>
  <div class="weater-box shadow-2xl flex justify-center rounded-lg">
    <div
      class="weater flex items-center justify-center font-extrabold md:text-9xl text-6xl"
    >
      <span>{{Math.round(temperature)}}<sup>o</sup>C</span>
    </div>
  </div>
</template>

<script>
  export default {
    props : ['temperature']
  }
 
</script>

<style scoped>
.weater-box {
  width: 50%;
  height: 200px;
  background: rgba(118, 131, 148, 0.8);
  margin: 50px auto;
}

@media(max-width: 768px) {
  .weater-box {
  height: 120px;
    
  }
}
</style>