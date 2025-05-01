<template>
    <div class="flex flex-col h-full  p-4  rounded bg-gray-950">
        <div class="flex items-center justify-between">
            <div class=""></div>
            <div class="flex gap-2 items-center">
                <img src="../public/logo5.png" alt="" class="img w-16 h-16">
                <h3 class="hidden md:block text-base font-semibold leading-6 text-gray-900 dark:text-white">
                    NearbyLocator
                </h3>
            </div>
            <UIcon name="i-heroicons-x-mark" class="size-5 hover:bg-slate-500"  @click="close" />

        </div>

        <!-- Body -->
        <div class="flex-1 p-4 space-y-4 overflow-auto">
            <USelect v-model="selectedCategory" :options="categoryOptions" option-attribute="label"
                value-attribute="key" placeholder="Select Category" size="xl" class="w-full"
                icon="i-heroicons-building-storefront-20-solid" />

            <USelect v-model="selectedService" :options="filteredServices" option-attribute="label"
                value-attribute="key" placeholder="Select Service" size="xl" class="w-full"
                :disabled="!selectedCategory" icon="i-heroicons-squares-plus-20-solid" />

            <LocationSearchInput @location-selected="handleLocationSelected"/>

            <div class="pt-4 text-sm text-gray-500">
                <strong>Selected:</strong>
                <pre>{{ selectedCategory }} / {{ selectedService }}</pre>
            </div>
            <div class="pt-4 text-sm text-gray-500 mt-52 pa">
                <strong>Marker Icon:</strong>
            </div>
            <div class="flex py-12 px-2 justify-center">
                <template v-if="selectedCategory">
                    <img :src="stringUrl" alt="icon" class="w-48 h-48 ">
                </template>
                <template v-else>
                    <img src="/logo1.png" alt="icon" class="w-48 h-48 m-auto opacity-15">
                </template>
            </div>
        </div>

        <!-- Footer -->
        <div class="p-4 border-t border-gray-600">
            <UButton label="Apply Filters" block @click="applyFilter" />
        </div>
    </div>
</template>

<script setup>
import { ref, computed, defineEmits, onMounted } from 'vue'


const emit = defineEmits(['close', 'update-service'])

// Drawer control
const props = defineProps({
    service: String,
    category: String
});
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
const selectedLocation = ref(null)
const close = () => {
    emit('close')
}

onMounted(() => {
    selectedCategory.value = props.category
    selectedService.value = props.service
    selectedLocation.value = null
})

// Category options (unique)
const categoryOptions = [
    { key: 'amenity', label: 'Amenity' },
    { key: 'tourism', label: 'Tourism' },
    { key: 'shop', label: 'Shop' },
    { key: 'leisure', label: 'Leisure' },
    { key: 'public_transport', label: 'Public Transport' },
    { key: 'railway', label: 'Railway' }
]

const handleLocationSelected = (location) => {
  selectedLocation.value = location;
  // Use it to update map or filter logic
}
// Filtered services based on selected category
watch(selectedService, () => {
    stringUrl.value = `/markers/${selectedService.value}.png`
}, { immediate: true })

const applyFilter = () => {
    emit('update-service', { selectedCategory: selectedCategory.value, selectedService: selectedService.value, selectedLocation: selectedLocation.value })
}

const filteredServices = computed(() =>
    services
        .filter(service => service.category === selectedCategory.value)
        .map(service => ({
            key: service.key,
            label: service.label
        }))
)
</script>