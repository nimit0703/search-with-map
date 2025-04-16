<template>
  <div class="flex">
    <ProductSearch
      :hotels="hotels" 
      :loading="loading" 
      @search-hotels="searchHotels" 
      @clear-hotels="clearHotels" 
      @focus-hotel="focusHotel"s
      class="h-screen"
    />
    
    
    <HotelMap 
    :hotels="hotels" 
    :map-center="mapCenter" 
    :map-zoom="mapZoom" 
    @map-move="handleMapMove"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';

const hotels = ref([]);
const loading = ref(false);
const mapCenter = ref([28.6139, 77.2090]); // Default to Delhi
const mapZoom = ref(14);


const searchHotels = async (city) => {
  loading.value = true;
  const coordinates = await getCoordinates(city);
  if (coordinates) {
    hotels.value = await getHotels(coordinates.lat, coordinates.lon);
  }
  loading.value = false;
};
onMounted(()=>{
  searchHotels("delhi");
})
const clearHotels = () => {
  hotels.value = [];
};

const focusHotel = (hotel) => {
  // Logic to focus on a specific hotel
};

const handleMapMove = async (moveData) => {
  loading.value = true;
  const { lat, lon, zoom } = moveData;
  mapCenter.value = [lat, lon];
  mapZoom.value = zoom;
  try {
    hotels.value = await getHotels(lat, lon);
  } catch (error) {
    
  }finally{
    loading.value = false;
  }
};

// Helper functions for coordinates and hotel fetching
const getCoordinates = async (city) => {
  const response = await fetch(`https://nominatim.openstreetmap.org/search?q=${city}&format=json`);
  const data = await response.json();
  if (data && data.length > 0) {
    const { lat, lon } = data[0];
    return { lat: parseFloat(lat), lon: parseFloat(lon) };
  }
  return null;
};

const getHotels = async (lat, lon, limit = 25) => {
  const query = `
    [out:json];
    (
      node["amenity"="fuel"](around:5000,${lat},${lon});
      way["amenity"="fuel"](around:5000,${lat},${lon});
      relation["amenity"="fuel"](around:5000,${lat},${lon});
    );
    out body;
  `;

  const response = await fetch(`https://overpass-api.de/api/interpreter?data=${encodeURIComponent(query)}`);
  const data = await response.json();

  // Filter only results that have lat/lon and limit results
  return data.elements.filter(hotel => hotel.lat && hotel.lon);
};
</script>

<style scoped>
/* Add custom styles */
</style>
