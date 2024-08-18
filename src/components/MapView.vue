<template>
  <div id="map" style="height: 500px;"></div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import icon from '../assets/marker.jpg'

export default {
  data() {
    return {
      map: null,
      marker: null,
      circle: null,
      radius: 1000, // 1 km
      iconUrl: icon // Path to your custom marker icon
    };
  },
  mounted() {
    this.initializeMap();
  },
  methods: {
    initializeMap() {
      this.map = L.map('map').setView([51.505, -0.09], 13);

      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(this.map);

      // Define custom marker icon
      const customIcon = L.icon({
        iconUrl: this.iconUrl,
        iconSize: [32, 32], // Size of the icon
        iconAnchor: [16, 32], // Point of the icon which will correspond to marker's location
        popupAnchor: [0, -32] // Point from which the popup should open relative to the iconAnchor
      });

      // Add marker with custom icon
      this.marker = L.marker([51.505, -0.09], { icon: customIcon }).addTo(this.map);
      this.circle = L.circle([51.505, -0.09], { radius: this.radius }).addTo(this.map);
    },
    updateLocation(lat, lng) {
      this.marker.setLatLng([lat, lng]);
      this.circle.setLatLng([lat, lng]);
      this.map.setView([lat, lng], 13);
    },
    isOutsideGeofence(lat, lng) {
      const distance = this.map.distance([lat, lng], [this.circle.getLatLng().lat, this.circle.getLatLng().lng]);
      return distance > this.circle.getRadius();
    },
    checkGeofence(lat, lng) {
      if (this.isOutsideGeofence(lat, lng)) {
        this.notifyUser();
      }
    },
    // notifyUser() {
    //   if (Notification.permission === 'granted') {
    //     console.log("You are outside the geofenced area!")
    //     new Notification('You are outside the geofenced area!');
    //   } else if (Notification.permission !== 'denied') {
    //     Notification.requestPermission().then(permission => {
    //       if (permission === 'granted') {
    //         console.log("You are outside the geofenced area!")
    //         new Notification('You are outside the geofenced area!');
    //       }
    //     });
    //   }
    // },
    // startGeofenceChecks() {
    //   this.checkIntervalId = setInterval(() => {
    //     this.checkGeofence();
    //   }, this.checkInterval);
    // },
    // stopGeofenceChecks() {
    //   if (this.checkIntervalId) {
    //     clearInterval(this.checkIntervalId);
    //   }
    // }
  },
  // beforeDestroy() {
  //   this.stopGeofenceChecks(); // Clean up the interval when the component is destroyed
  // }
};
</script>

<style scoped>
#map {
  height: 100%;
}
</style>
