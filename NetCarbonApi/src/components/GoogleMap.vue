<template>
  <div class="component-container">
    <h2 style="text-align: center;"> Launchpads and Landpads Positions</h2>
    <br>
    <GMapMap
      :center="center"
      :zoom="7"
      map-type-id="terrain"
      style="width: 500px; height: 300px; border-radius: 10px;"
    >
      <GMapCluster>
        <GMapMarker
          :key="m.key"
          v-for="(m, index) in markers"
          :position="m.position"
          :clickable="m.clickable"
          :draggable="m.draggable"
          @click="center = m.position"
        />
      </GMapCluster>
    </GMapMap>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      center: { lat: 44.8426893, lng: -0.5711412 },  // Centre sur Bordeaux
      markers: [],
      launchpads: [],
      landpads: []
    };
  },
  mounted() {
    this.fetchLaunchpads();
    this.fetchLandpads();
  },
  methods: {
    async fetchLaunchpads() {
      try {
        const response = await axios.get('https://api.spacexdata.com/v4/launchpads');
        this.launchpads = response.data;

        this.updateMarkers();
      } catch (error) {
        console.error(error);
      }
    },
    async fetchLandpads() {
      try {
        const response = await axios.get('https://api.spacexdata.com/v4/landpads');
        this.landpads = response.data;

        this.updateMarkers();
      } catch (error) {
        console.error(error);
      }
    },
    updateMarkers() {
      const launchpadMarkers = this.launchpads.map((launchpad, index) => {
        return {
          key: `launchpad_${index}`,
          position: {
            lat: launchpad.latitude,
            lng: launchpad.longitude
          },
          clickable: true,
          draggable: true
        };
      });

      const landpadMarkers = this.landpads.map((landpad, index) => {
        return {
          key: `landpad_${index}`,
          position: {
            lat: landpad.latitude,
            lng: landpad.longitude
          },
          clickable: true,
          draggable: true
        };
      });

      this.markers = [...launchpadMarkers, ...landpadMarkers];
    }
  }
};
</script>

<style>
.component-container {
  background-color: #f2f2f2;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  margin-right: 10px;
}
</style>
