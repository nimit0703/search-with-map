<template>
    <div class="h-screen flex ">
        <!-- Header / Top bar -->
        <div class="flex items-start p-2">
            <UButton label="Open Drawer" color="neutral" variant="subtle" @click="isDrawerOpen = true">
                <UIcon name="i-lucide-menu" class="size-5" />
            </UButton>
        </div>

        <!-- Scrollable content area -->
        <div class="flex-1 overflow-y-auto ">
            <div class="grid grid-cols-4 gap-2  h-full ">
                <div class="hidden lg:block">
                    <!-- Listing part -->
                    <ProductList :products="products" />
                </div>
                <div class="col-span-4 lg:col-span-3">
                    <!-- Map Part -->
                    <GlobalMap :map-center="mapCenter" :map-zoom="mapZoom" @map-move="handleMapMove"
                        :products="products" :icon="selectedService" />
                </div>
            </div>
        </div>
    </div>


    <USlideover v-model="isDrawerOpen" title="Dropdown in Drawer" prevent-close side="left" class="z-[9999]">
        <SlideoverContent @close="isDrawerOpen = false" @update-service="updateService" :service="selectedService"
            :category="selectedCategory" />
    </USlideover>
</template>

<script setup>
import { ref, computed } from 'vue'

// Drawer control
const isDrawerOpen = ref(false)
const mapCenter = ref([28.6139, 77.2090]); // Default to Delhi
const mapZoom = ref(14);
// Category selection
const selectedCategory = ref('amenity')
const selectedService = ref('fuel')
const stringUrl = ref('')
// Main services with category
const services = [
    { category: 'amenity', key: 'fuel', label: 'Gas Station' },
    { category: 'amenity', key: 'charging_station', label: 'EV Charging Station' },
    { category: 'amenity', key: 'bicycle_rental', label: 'Bicycle Rental' },
    { category: 'tourism', key: 'hotel', label: 'Hotel' },
    { category: 'tourism', key: 'guest_house', label: 'Guest House' },
    { category: 'tourism', key: 'hostel', label: 'Hostel' },
    { category: 'amenity', key: 'restaurant', label: 'Restaurant' },
    { category: 'amenity', key: 'cafe', label: 'Cafe' },
    { category: 'amenity', key: 'fast_food', label: 'Fast Food' },
    { category: 'shop', key: 'supermarket', label: 'Supermarket' },
    { category: 'shop', key: 'clothes', label: 'Clothing Store' },
    { category: 'leisure', key: 'park', label: 'Park' },
    { category: 'leisure', key: 'playground', label: 'Playground' },
    { category: 'amenity', key: 'hospital', label: 'Hospital' },
    { category: 'amenity', key: 'pharmacy', label: 'Pharmacy' },
    { category: 'amenity', key: 'atm', label: 'ATM' },
    { category: 'amenity', key: 'toilets', label: 'Public Toilets' },
    { category: 'amenity', key: 'drinking_water', label: 'Water Station' },
    { category: 'tourism', key: 'museum', label: 'Museum' },
    { category: 'public_transport', key: 'bus_stop', label: 'Bus Stop' },
    { category: 'railway', key: 'station', label: 'Train Station' }
]

const products = ref([])

const mapCords = ref({})

const updateService = (data) => {
    selectedService.value = data.selectedService
    selectedCategory.value = data.selectedCategory
    isDrawerOpen.value = false
    handleMapMove(mapCords.value)
}
// Category options (unique)
const categoryOptions = [
    { key: 'amenity', label: 'Amenity' },
    { key: 'tourism', label: 'Tourism' },
    { key: 'shop', label: 'Shop' },
    { key: 'leisure', label: 'Leisure' },
    { key: 'public_transport', label: 'Public Transport' },
    { key: 'railway', label: 'Railway' }
]

const handleMapMove = async (moveData) => {
    const { lat, lon, zoom } = moveData;
    mapCords.value = { lat, lon, zoom }
    mapCenter.value = [lat, lon];
    mapZoom.value = zoom;
    try {
        products.value = await getProducts(lat, lon);
    } catch (error) {

    } finally {
    }
};

const getProducts = async (lat, lon, limit = 25) => {
    const query = `
    [out:json];
    (
      node["${selectedCategory.value}"="${selectedService.value}"](around:5000,${lat},${lon});
      way["${selectedCategory.value}"="${selectedService.value}"](around:5000,${lat},${lon});
      relation["${selectedCategory.value}"="${selectedService.value}"](around:5000,${lat},${lon});
    );
    out body;
  `;
    const response = await fetch(`https://overpass-api.de/api/interpreter?data=${encodeURIComponent(query)}`);
    const data = await response.json();

    // Filter only results that have lat/lon and limit results
    return data.elements.filter(product => product.lat && product.lon);
}

// Filtered services based on selected category
watch(selectedService, () => {
    stringUrl.value = `/markers/${selectedService.value}.png`
}, { immediate: true })
const filteredServices = computed(() =>
    services
        .filter(service => service.category === selectedCategory.value)
        .map(service => ({
            key: service.key,
            label: service.label
        }))
)
</script>