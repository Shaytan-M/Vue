<script setup>
import { onMounted } from 'vue';
import HelloWorld from '../components/MapWrapper.vue'
</script>
<template>
  <div class="mapWrapper">
    <HelloWorld/>
  </div>
</template>

<style lang="less">
  #map{
    height: 800px;
  }
</style>
