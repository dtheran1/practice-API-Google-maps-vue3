<template>
  <div class="d-flex text-center" style="height: 20vh">
    <div class="m-auto">
      <h4>Your Position</h4>
      Latitude: {{ currPos.lat.toFixed(2) }}, Longitude:
      {{ currPos.lng.toFixed(2) }}
    </div>
  </div>
  <div ref="mapDiv" style="width: 100%; height: 80vh;"></div>
</template>

<script>
/* eslint-disable no-undef */
import { computed, onMounted, ref } from "vue";
import { useGeolocation } from "./useGeolocation.js";

import { Loader } from "@googlemaps/js-api-loader";

const GOOGLE_MAPS_API_KEY = "AIzaSyAbISdzV4THYbAgp6U1P0b5BpfMgxyM9lU";
export default {
  name: "App",
  components: {},
  setup() {
    const { coords } = useGeolocation();
    const currPos = computed(() => ({
      lat: coords.value.latitude,
      lng: coords.value.longitude,
    }));

    const loader = new Loader({ apiKey: GOOGLE_MAPS_API_KEY });
    const mapDiv = ref(null);
    onMounted(async () => {
      await loader.load();
      new google.maps.Map(mapDiv.value, {
        center: currPos.value, 
        zoom: 15
      });
    });

    return { currPos, mapDiv };
  },
};
</script>
