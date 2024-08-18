<template>
  <div id="app">
    <GeoLocation @location-updated="updateLocation" @check-geofence="checkGeofence" />
    <MapView ref="mapComponent" />
  </div>
</template>

<script>
import GeoLocation from './components/GeoLocation.vue';
import MapView from './components/MapView.vue';

export default {
  name: 'App',
  components: {
    MapView,
    GeoLocation
  },
  mounted() {
    // this.$refs.mapComponent.startGeofenceChecks(); // Start checking geofence periodically
  },
  // beforeDestroy() {
  //   this.$refs.mapComponent.stopGeofenceChecks(); // Clean up on destroy
  // },
  methods: {
    updateLocation(lat, lng) {
      this.$refs.mapComponent.updateLocation(lat, lng);
    },
    checkGeofence(lat, lng) {
      this.$refs.mapComponent.checkGeofence(lat, lng);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
