<template>
    <div class="flex flex-col gap-4 py-4 pr-2">

        <div v-for="item in products" :key="item.id">
            <UCard>
                <div class="grid grid-cols-3 gap-4 items-start">
                    <img :src="`/markers/${props.icon}.png`" class="border rounded-md py-8 " />
                    <div class="grid grid-cols-3 gap-4 col-span-2 items-center">

                        <div class="flex flex-col">
                            <h1 v-if="item?.tags.name"><strong>Name </strong></h1>
                            <h1 v-if="item?.tags.amenity"><strong>Category</strong></h1>
                            <h1 v-if="item?.tags.brand"><strong>Brand</strong></h1>
                            <h1 v-if="item?.tags.internet_access"><strong>Internet</strong></h1>
                            <h1 v-if="item?.tags.takeaway"><strong>takeaway</strong></h1>
                            <h1 v-if="item?.tags.cuisine"><strong>cuisine</strong></h1>
                        </div>
                        <div class="flex flex-col col-span-2">
                            <h1 class="line-clamp-1" v-if="item?.tags.name">: {{ item?.tags.name || "Unknown" }}</h1>
                            <h1 class="line-clamp-1" v-if="item?.tags.amenity">: {{ item?.tags.amenity || "Unknown" }}
                            </h1>
                            <h1 class="line-clamp-1" v-if="item?.tags.brand">: {{ item?.tags.brand || "Unknown" }}</h1>
                            <h1 class="line-clamp-1" v-if="item?.tags.internet_access">: {{ item?.tags.internet_access ||
                                "Unknown" }}</h1>
                            <h1 class="line-clamp-1" v-if="item?.tags.takeaway">: {{ item?.tags.takeaway || "Unknown" }}
                            </h1>
                            
                        </div>
                    </div>
                </div>
            </UCard>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    products: [],
    icon: String
})
</script>