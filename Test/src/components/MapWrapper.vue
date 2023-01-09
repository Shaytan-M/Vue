<script setup>
  import { defineProps, onActivated, ref } from 'vue';
    let inpName = ref('');
    let inpLat = ref('');
    let inpLng = ref('');
    let createMarkerOnMap = () => {};
    import("leaflet").then((leaflet) => {
      // Create map
      let map = leaflet.map('map');
      leaflet.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 250,
      attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
      }).addTo(map);

       // Create marker on map
      createMarkerOnMap = (e, status) => {
        let marker = leaflet.marker([e.latlng.lat, e.latlng.lng]).addTo(map);
        marker.bindPopup(`${e.name ? `<b>${e.name}</b><br>` : ''}(${e.latlng.lat}, ${e.latlng.lng})`).openPopup();
        marker.showPopup = false;
        marker.on('mouseover', (e) => {
          marker.openPopup()
        });
        marker.on('mouseout', (e) => {
          if(!marker.showPopup){
            marker.closePopup()
          }
        });
        marker.on('click', (e) => {
          marker.openPopup()
          marker.showPopup = !marker.showPopup;
        });
        if (status == 'new') {
          map.setView([e.latlng.lat, e.latlng.lng], 6)
        }
      }

      // Create default marker
      if(localStorage.getItem('Markers')){
        let lstorage = JSON.parse(localStorage.getItem('Markers'));
        lstorage.map(item => createMarkerOnMap(item))
      }
      map.setView([49.482, 31.695], 6)
      map.on('click', onMapClick);
    })
    // Add marker data in localStorage
    function addMerkerData(markerData){
        let arr = [];
        let storage = localStorage.getItem('Markers');
        if(storage){
          arr = JSON.parse(storage)
          arr.push(markerData)
          localStorage.setItem('Markers', JSON.stringify(arr))
        }else{
          arr.push(markerData)
          localStorage.setItem('Markers', JSON.stringify(arr))
        }
    }
     // Event map click
     function onMapClick(e){
        let nameMarker = e.name ?? prompt('Вкажіть назву макркеру');
        let data = {
          latlng: {
            lat:e.latlng.lat,
            lng:e.latlng.lng,
          },
          name: nameMarker,
        }
        createMarkerOnMap(data, 'new')
        addMerkerData(data)
      }
    
      // Create Marker from form
      const createMarker = (e) => {
        e.preventDefault();
        let obj = {
          name: inpName.value,
          latlng: {
            lat: inpLat.value,
            lng: inpLng.value,
          },

        }
        console.log();
        onMapClick(obj)
      }

</script>

<template>
  <div class="search">
        <form>
          <input type="text" placeholder="Name" v-model="inpName">
          <input type="number" required placeholder="Latitude" v-model="inpLat">
          <input type="number" required placeholder="Longitude" v-model="inpLng">
          <button class="default-btn" @click="(e) => createMarker(e)">Create Marker</button>
        </form>
    </div>
  <div id="map">
    
  </div>
</template>

<style scoped lang="less">
  .search{
    form{
      padding: 10px 5px;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      grid-gap: 20px;

      input{
          padding: 5px 10px;
          border-radius: 7px;
          background-color: #383838;
          border: 0;
          font-size: 14px;
          color: white;
      }
      input::-webkit-outer-spin-button,
      input::-webkit-inner-spin-button {
          /* display: none; <- Crashes Chrome on hover */
          -webkit-appearance: none;
          margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
      }
    }
  }
</style>
