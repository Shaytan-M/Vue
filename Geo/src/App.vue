<script setup>
import { RouterLink, RouterView, useRoute } from 'vue-router'
import { useStore } from './stores/mainStore'
import { computed } from 'vue';

let modalActive = computed(() => {
  return useStore().modalShow
})

</script>

<template>
  <div id="main" :class="modalActive ? 'modal-active' : ''">
    <header>
    <div class="wrapper">
        <RouterLink class="btn" :class="useRoute().path == '/' ? 'active' : ''" to="/">Home</RouterLink>
        <RouterLink class="btn" :class="useRoute().path == '/map' ? 'active' : ''" to="/map">Map</RouterLink>
    </div>
  </header>
  <RouterView />
  </div>
</template>

<style scoped lang="less">  
  .wrapper{
    padding: 10px 0;
    text-align: center;
    .active{
      border-bottom: 2px solid hsla(160, 100%, 37%, 1);
    }
  }
  #main.modal-active{
    overflow: hidden;
    max-height: 100vh;
  }
</style>
