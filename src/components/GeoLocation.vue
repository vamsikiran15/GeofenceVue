<template>
  <div>
    <button @click="startTracking">Start Tracking</button>
    <button @click="stopTracking">Stop Tracking</button>
    <p v-if="location">Current Location: {{ location.lat }}, {{ location.lng }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: null,
      watchId: null
    };
  },
  methods: {
    startTracking() {
      if (navigator.geolocation) {
        this.watchId = navigator.geolocation.watchPosition(
          position => {
            this.location = {
              lat: position.coords.latitude,
              lng: position.coords.longitude
            };
            this.$emit('location-updated', this.location.lat, this.location.lng);
            this.$emit('check-geofence', this.location.lat, this.location.lng); // Emit event for geofence check
          },
          error => {
            console.error('Error getting location:', error);
          },
          { enableHighAccuracy: true }
        );
      } else {
        console.error('Geolocation is not supported by this browser.');
      }
    },
    stopTracking() {
      if (this.watchId !== null) {
        navigator.geolocation.clearWatch(this.watchId);
        this.watchId = null;
      }
    }
  }
};
</script>
