<template>
  <div class="relative shadow-lg shadow-black" :class="colapse ? 'w-12':'md:w-1/3'">
    <div class="p-4 justify-center w-full" >
        <input
            v-model="city"
            @input="searchHotels"
            placeholder="Enter city name"
            class="w-[90%] p-3 border rounded mb-4"
            v-if="!colapse"
        />
       
      <!-- Skeleton Loader -->
      <div v-if="loading && !colapse" class="space-y-4 overflow-y-auto h-[86vh] px-2">
        <div class="" v-for="i in 6" :key="i" >
          <div class="">
            <div class="skeleton-card"></div>
          </div>
    
        </div>
      </div>
  
      <!-- Hotels Listing -->

      <div v-else-if="hotels.length && !colapse" class="space-y-4 overflow-y-auto h-[86vh] px-2" >
        <ul>
          <li
            v-for="hotel in hotels"
            :key="hotel.id"
            class="cursor-pointer border p-3 rounded-lg shadow hover:bg-gray-100 my-2"
            @click="focusHotel(hotel)"
          >
            <div class="flex">
              <!-- Hotel Image -->
              <img
                v-if="hotel.tags.image"
                :src="hotel.tags.image"
                alt="Hotel Image"
                class="w-24 h-24 rounded-lg mr-4 object-cover"
              />
              <img
                v-else
                src="https://st2.depositphotos.com/12982378/47876/i/450/depositphotos_478760220-stock-photo-cropped-view-woman-holding-petrol.jpg"
                alt="Hotel Image"
                class="w-24 h-24 rounded-lg mr-4 object-cover"
              />
  
              <div class="flex-1 group">
                <h3 class="font-semibold text-lg group-hover:text-black">{{ hotel.tags.name || 'Fuel Station' }}</h3>
                <p class="text-sm text-gray-500">{{ hotel.tags.description || 'No description available' }}</p>
                <p class="text-sm text-gray-400">{{ hotel.address || 'No address provided' }}</p>
                <div class="flex justify-between items-center mt-2">
                  <span class="text-sm text-gray-500">Opening hours: {{ hotel.tags.opening_hours || '' }}</span>
                  <span class="text-sm text-gray-500">{{ hotel.tags.brand || '' }}</span>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
  
      <!-- No Hotels Found -->
      <div v-else-if="!colapse">
        <p class="text-gray-500">No hotels found</p>
      </div>
    </div>
    <button class="absolute right-0 w-12 top-5 pl-2" style="z-index: 999;" @click="toggle">
      <UIcon :name="!colapse? 'i-heroicons:chevron-double-left-16-solid' :'i-heroicons:chevron-double-right-16-solid'" class=" text-3xl"> </UIcon>
    </button>
  </div>

  </template>
  
  <script setup>
  import { ref, defineEmits } from 'vue';
  const colapse = ref('false')
  const emit = defineEmits(['search-hotels', 'clear-hotels', 'focus-hotel']);
  
  // Props to get the hotel data and focus function
  const props = defineProps({
    hotels: Array,
    focusHotel: Function,
    loading: Boolean,
  });
  
  const city = ref('delhi');
  
  // Search hotels when city is entered
  const searchHotels = async () => {
    if (city.value.length >= 3) {
      emit('search-hotels', city.value);
    } else {
      emit('clear-hotels');
    }
  };
  const toggle = () =>{
    colapse.value = !colapse.value; 
  }
  </script>
  
  <style scoped>
  /* Skeleton Loader Style */
  .skeleton-card {
    background: #6968683e;
    height: 120px;
    width: 220px;
    border-radius: 8px;
    margin-bottom: 10px;
    animation: skeleton-loading 1.2s infinite ease-in-out;
  }
  
  @keyframes skeleton-loading {
    0% {
      background-color: #e0e0e041;
    }
    50% {
      background-color: #d1d1d13b;
    }
    100% {
      background-color: #e0e0e016;
    }
  }
  
  /* Optional: Add custom card styles */
  .skeleton-card .image-placeholder {
    height: 100%;
    width: 100%;
    background: #ddd;
    border-radius: 8px;
  }
  </style>
  