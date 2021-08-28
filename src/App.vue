<template>
  <nav class="uk-navbar-container">
    <div class="uk-navbar-left uk-background-primary uk-light uk-padding ">
      <a class="uk-navbar-item uk-logo">IP Address Locator</a>
      <div class="uk-navbar-item">
        <form action="javascript:void(0)">
          <input class="uk-input uk-form-width-medium"
          v-model="queryIp"
          v-on:keyup.enter="getIpInfo"
          type="text" placeholder="Enter IP address to search">
        </form>
      </div>
    <ip-panel v-if="ipInfo" v-bind:ipInfo="ipInfo"/>
    </div>
  </nav>
    <div id="mapid" class="z-10" style='width: 100%; height: 600px;'></div>
</template>

<script>
import IpPanel from './components/IpPanel.vue'
import leaflet from 'leaflet'
import { onMounted, ref } from 'vue'
import axios from 'axios'

export default {
  name: 'Home',
  components: { IpPanel },

  setup(props) {
    let mymap

    const queryIp = ref('')
    const ipInfo = ref(null)

    onMounted(() => {
      mymap = leaflet.map('mapid').setView([40.7797156,-73.947738], 9)
      leaflet
        .tileLayer(
          'https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiZ29zbWl0aCIsImEiOiJja3NzYnFhY2kwdXppMzBxa2t1ajVmenJ4In0.tJDIFtqUxR4s5FBXUF-GWQ',
          {
            attribution:
              'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
            maxZoom: 18,
            id: 'mapbox/streets-v11',
            tileSize: 512,
            zoomOffset: -1,
            accessToken:
              'pk.eyJ1IjoiZ29zbWl0aCIsImEiOiJja3NzYnFhY2kwdXppMzBxa2t1ajVmenJ4In0.tJDIFtqUxR4s5FBXUF-GWQ',
          }
        )
        .addTo(mymap)
    })

    const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `https://geo.ipify.org/api/v1?apiKey=at_FBTi0rGPhQlKp2cRdOJV0t3Mibq6F&ipAddress=${queryIp.value}`
        )
        const result = data.data
        ipInfo.value = {
          address: result.ip,
          state: result.location.region,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.lat,
          lng: result.location.lng,
        }
        leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(mymap)
        mymap.setView([ipInfo.value.lat, ipInfo.value.lng], 13)
      } catch (err) {
        alert(err.message)
      }console.log(ipInfo)
    }

    return { queryIp, ipInfo, getIpInfo }
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
