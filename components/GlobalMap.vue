<template>
    <div id="map" class="w-full h-auto">
    </div>
</template>
<script setup>
import { onMounted, ref, defineEmits } from 'vue';

  let L;
  const emit = defineEmits(['map-move']);

  const map = ref(null);
const markers = ref([]);
const props = defineProps({
  icon:String,
  products: Array,
  mapCenter: Array,
  mapZoom: Number
});
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

    // Show initial products on the map
  }
});

watch(() => props.products, (newProducts) => {
  showProductsOnMap(newProducts);
}, { immediate: false });


const showProductsOnMap = (productData) => {
  if (!map.value) return;

  // Clear existing markers
  markers.value.forEach(marker => {
    marker.remove()
  });
  markers.value = [];
  const fuelIcon = L.icon({
    iconUrl: `/markers/${props.icon}.png`, // Fuel station icon
    iconSize: [30, 30], // Size of the icon
    iconAnchor: [15, 30], // Anchor point of the icon
    popupAnchor: [0, -30] // Position of popup relative to the icon
  });
  productData.forEach(product => {
    const productLat = product?.lat || product?.center?.lat;
    const productLon = product?.lon || product?.center?.lon;
    if (!productLat || !productLon) return;

    const marker = L.marker([productLat, productLon], { icon: fuelIcon }).addTo(map.value);
    marker.bindPopup(`<b>${product.tags.name || 'Unnamed Product'}</b><br>${product.tags.description || 'No description'}`);
    markers.value.push(marker);
  });
};
const handleMapMove = async () => {
  const center = map.value.getCenter();
  const zoom = map.value.getZoom();
  emit('map-move', { lat: center.lat, lon: center.lng, zoom });
};

</script>

<style>
#map {
  height: 100%;
  width: 100%;
  margin: auto;
}</style>
