<template>
  <div class="icon-box flex justify-center items-center flex-col -mt-8">
    <div class="text-2xl font-bold font-mono">{{ text }}</div>
     
    <div class="-mt-4">
      <img :src="icon" alt="" class="w-40 h-40">

    </div>

  </div>
</template>

<script>
export default {
  props : ['icon', 'text']
}
 


</script>