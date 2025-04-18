<template>
    <div class="flex flex-col gap-4 py-4 pr-2 justify-end items-end">
        <div class="flex">
            <h1 class="italic mr-2 text-gray-400"> Total Products</h1>
            <h1>{{ products.length || 'Null' }}</h1>
        </div>
        <div v-for="item in products" :key="item.id">
            <div class="flex w-full h-[220px] py-2">
                <div class="w-20"></div>
                <div class="flex-1 rounded-xl relative shadow-sm shadow-gray-200 flex items-center">
                    <div
                        class="absolute w-32 h-40 rounded-xl left-[-48px] bg-slate-800 shadow-slate-400 shadow-sm flex items-center justify-center">
                        <img :src="`/markers/${props.icon}.png`" class="rounded-md w-28 h-32" />
                    </div>
                    <div class="ml-[92px] mt-4">
                        <div class="grid grid-cols-3 gap-4 col-span-2 items-center">

                            <div class="flex flex-col">
                                <h1 class="text-gray-400 opacity-90 italic"><strong>Name </strong></h1>
                                <h1 class="text-gray-400 opacity-90 italic"><strong>Category</strong></h1>
                                <h1 class="text-gray-400 opacity-90 italic"><strong>Brand</strong></h1>
                                <h1 class="text-gray-400 opacity-90 italic"><strong>Internet</strong></h1>
                                <h1 class="text-gray-400 opacity-90 italic"><strong>takeaway</strong></h1>
                                <h1 class="text-gray-400 opacity-90 italic"><strong>cuisine</strong></h1>
                            </div>
                            <div class="flex flex-col col-span-2">
                                <h1 class="line-clamp-1 capitalize text-gray-100 font-sans">: {{ item?.tags.name ||
                                    "Unknown" }}
                                </h1>
                                <h1 class="line-clamp-1 capitalize text-gray-100 font-sans">: {{ item?.tags.amenity ||
                                    "Unknown"
                                    }}
                                </h1>
                                <h1 class="line-clamp-1 capitalize text-gray-100 font-sans">: {{ item?.tags.brand ||
                                    "Unknown" }}
                                </h1>
                                <h1 class="line-clamp-1">: {{
                                    item?.tags.internet_access ||
                                    "Unknown" }}</h1>
                                <h1 class="line-clamp-1 capitalize text-gray-100 font-sans">: {{ item?.tags.takeaway ||
                                    "Unknown" }}
                                </h1>
                                <h1 class="line-clamp-1 capitalize text-gray-100 font-sans">: {{ item?.tags.cuisine ||
                                    "Unknown" }}
                                </h1>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    products: [],
    icon: String
})
</script>