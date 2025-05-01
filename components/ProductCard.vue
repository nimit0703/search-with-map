<template>
    <div class="p-2 w-full">
        <div class="w-full border-black px-2 py-5 rounded-2xl min-h-48  shadow-black shadow-md">
            <div class="flex  align-middle">
                <div class="w-36 h-36  p-1 mt-2">
                    <img :src="`/images/${getRandomNumber(20, 40)}.jpg`" alt=""
                        class="rounded-3xl h-36 w-36 shadow-black shadow-md hover:shadow-black hover:shadow-lg">
                </div>
                <div class="flex-1 p-1">
                    <div class="flex items-center gap-2 justify-between">
                        <div class="flex flex-col">
                            <div class="text-wrap line-clamp-2">
                                <h1>{{ product?.name || "Unknown" }}</h1>
                            </div>
                            <div class="flex align-bottom items-center">
                                <UIcon :name="'i-heroicons:map-pin'" class="text-gray-500"> </UIcon>
                                <span class="text-gray-500"> {{ product?.brand || "address" }}</span>
                            </div>
                        </div>
                        <div class="flex">
                            <HeartFilled/>

                            <UIcon :name="'i-heroicons:share-16-solid'" class="text-xl"> </UIcon>
                        </div>
                    </div>
                    <div class="flex mt-2 gap-4">
                        <div class="flex gap-1 justify-center items-center">
                            <UIcon :name="'i-heroicons:users'" class=""> </UIcon>
                            <span class="text-sm">x{{ getRandomNumber(1, 15) }} Per</span>
                        </div>
                        <div class="flex gap-1 justify-center items-center">
                            <UIcon :name="'i-heroicons:user-group'" class=""> </UIcon>
                            <span class="text-sm">x{{ getRandomNumber(1, 20) }} Places</span>
                        </div>
                    </div>
                    <div class="flex justify-between mt-6 items-center">
                        <button class="bg-purple-500 px-4 py-2 rounded-2xl font-semibold">More details</button>
                        <div class="text-sm text-gray-500">
                            <span class="text-xl text-white">{{ getRandomNumber(1, 100) + "."
                                +getRandomNumber(1,99)}}$/</span>mat
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    product: Object,
    icon: String
})
const isFilled = ref(false);

const getRandomNumber = (min, max) => {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
</script>

<style lang="scss" scoped></style>