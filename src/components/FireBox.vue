<template>
    <div v-for="(_, wIndex) in sizedArray(fireWidth)" class="pixel-row">
        <div
            v-for="(_, hIndex) in sizedArray(fireHeight)"
            :style="{ backgroundColor: matrixMode ? 'transparent' : setPixelColor(wIndex, hIndex) }"
            class="pixel"
        >
            <div :style="{ color: setPixelColor(wIndex, hIndex) }">{{ firePixelArray[hIndex + (wIndex * fireHeight)] }}</div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, defineProps } from 'vue';

const { firePixelArray, fireHeight } = defineProps({
    firePixelArray: {
        type: Array,
        required: true
    },
    fireWidth: {
        type: Number,
        required: true
    },
    fireHeight: {
        type: Number,
        required: true
    },
    matrixMode: {
        type: Boolean,
        required: true
    },
})

const colorsPallete = ref([{"r":7,"g":7,"b":7},{"r":31,"g":7,"b":7},{"r":47,"g":15,"b":7},{"r":71,"g":15,"b":7},{"r":87,"g":23,"b":7},{"r":103,"g":31,"b":7},{"r":119,"g":31,"b":7},{"r":143,"g":39,"b":7},{"r":159,"g":47,"b":7},{"r":175,"g":63,"b":7},{"r":191,"g":71,"b":7},{"r":199,"g":71,"b":7},{"r":223,"g":79,"b":7},{"r":223,"g":87,"b":7},{"r":223,"g":87,"b":7},{"r":215,"g":95,"b":7},{"r":215,"g":95,"b":7},{"r":215,"g":103,"b":15},{"r":207,"g":111,"b":15},{"r":207,"g":119,"b":15},{"r":207,"g":127,"b":15},{"r":207,"g":135,"b":23},{"r":199,"g":135,"b":23},{"r":199,"g":143,"b":23},{"r":199,"g":151,"b":31},{"r":191,"g":159,"b":31},{"r":191,"g":159,"b":31},{"r":191,"g":167,"b":39},{"r":191,"g":167,"b":39},{"r":191,"g":175,"b":47},{"r":183,"g":175,"b":47},{"r":183,"g":183,"b":47},{"r":183,"g":183,"b":55},{"r":207,"g":207,"b":111},{"r":223,"g":223,"b":159},{"r":239,"g":239,"b":199},{"r":255,"g":255,"b":255}])

const sizedArray = (range: number) => Array.from({ length: range })

const setPixelColor = (wIndex: number ,hIndex: number) => {
    const fireIntensity: any = firePixelArray[hIndex + (wIndex * fireHeight)]

    const {r = '7', g = '7', b = '7'} = colorsPallete.value[fireIntensity] || {}

    return `rgb(${r}, ${g}, ${b})`
}
</script>

<style lang="scss">
.pixel-row {
    display: flex;

    .pixel {
        height: .563rem;
        width: .563rem;
        border: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        user-select: none;

        div {
            font-size: .46rem;
        }
    }
}
</style>
  