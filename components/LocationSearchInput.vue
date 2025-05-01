<template>
    <div class="relative">
      <UInput
        v-model="locationInput"
        icon="i-lucide-search"
        size="md"
        variant="outline"
        class="pl-1"
        placeholder="Search with location"
      />
  
      <div
        v-if="locationSuggestions.length"
        class="absolute z-50 w-full bg-white dark:bg-gray-800 mt-1 rounded shadow max-h-60 overflow-y-auto"
      >
        <div
          v-for="(suggestion, index) in locationSuggestions"
          :key="index"
          class="p-2 text-sm cursor-pointer hover:bg-gray-100 dark:hover:bg-gray-700"
          @click="selectLocationSuggestion(suggestion)"
        >
          {{ suggestion.display_name }}
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, watch } from 'vue'
  const emit = defineEmits(['location-selected'])
  
  const locationInput = ref('')
  const locationSuggestions = ref([])
  const debounceTimeout = ref(null)
  
  watch(locationInput, (newVal) => {
    if (debounceTimeout.value) clearTimeout(debounceTimeout.value)
  
    if (!newVal || newVal.length < 3) {
      locationSuggestions.value = []
      return
    }
  
    debounceTimeout.value = setTimeout(async () => {
      try {
        const res = await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(newVal)}&addressdetails=1&limit=5`)
        const data = await res.json()
  
        locationSuggestions.value = data.map(loc => ({
          display_name: loc.display_name,
          lat: loc.lat,
          lon: loc.lon
        }))
      } catch (e) {
        console.error('Nominatim autocomplete error:', e)
      }
    }, 400)
  })
  
  const selectLocationSuggestion = (suggestion) => {
    locationInput.value = suggestion.display_name
    locationSuggestions.value = []
    emit('location-selected', {
      name: suggestion.display_name,
      lat: parseFloat(suggestion.lat),
      lon: parseFloat(suggestion.lon)
    })
  }
  </script>
  