<template>
  <div id="map" class="w-full h-auto rounded-lg">
  </div>
</template>

<script setup>
import { onMounted, ref, defineEmits } from 'vue';
const emit = defineEmits(['map-move']);

// Props to receive map configuration and hotels
const props = defineProps({
  hotels: Array,
  mapCenter: Array,
  mapZoom: Number
});

let L;
const map = ref(null);
const markers = ref([]);

onMounted(async () => {
  if (process.client) {
    L = (await import('leaflet')).default; // Dynamically import Leaflet

    // Initialize the map
    map.value = L.map('map').setView(props.mapCenter, props.mapZoom);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map.value);

    // Watch for map movement
    map.value.on('moveend', handleMapMove);

    // Show initial hotels on the map
    showHotelsOnMap(props.hotels);
  }
});
watch(() => props.hotels, (newHotels) => {
  showHotelsOnMap(newHotels);
}, { immediate: false });
// Show hotels on the map by adding markers for each

const showHotelsOnMap = (hotelData) => {
  if (!map.value) return;

  // Clear existing markers
  markers.value.forEach(marker => {
    marker.remove()
  });
  markers.value = [];
  const fuelIcon = L.icon({
    iconUrl: 'fuelStation.png', // Fuel station icon
    iconSize: [30, 30], // Size of the icon
    iconAnchor: [15, 30], // Anchor point of the icon
    popupAnchor: [0, -30] // Position of popup relative to the icon
  });
  hotelData.forEach(hotel => {
    const hotelLat = hotel?.lat || hotel?.center?.lat;
    const hotelLon = hotel?.lon || hotel?.center?.lon;
    if (!hotelLat || !hotelLon) return;

    const marker = L.marker([hotelLat, hotelLon], { icon: fuelIcon }).addTo(map.value);
    marker.bindPopup(`<b>${hotel.tags.name || 'Unnamed Hotel'}</b><br>${hotel.tags.description || 'No description'}`);
    markers.value.push(marker);
  });
};

// Handle map movement (pan or zoom)
const handleMapMove = async () => {
  const center = map.value.getCenter();
  const zoom = map.value.getZoom();
  emit('map-move', { lat: center.lat, lon: center.lng, zoom });
};
</script>

<style scoped>
#map {
  height: 90vh;
  width: 90%;
  margin: auto;
}

.fade-out {
  opacity: 1;
  transition: opacity 0.5s ease-in-out;
}

.fade-out.leaflet-marker-icon {
  opacity: 0;
}
</style>
