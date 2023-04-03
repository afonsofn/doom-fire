<template>
  <main>
    <FireBox
      :fire-pixel-array="firePixelArray"
      :matrix-mode="matrixMode"
      :fire-height="fireHeight" 
      :fire-width="fireWidth"
    />

    <div class="buttons-wrapper">
      <ControlButtons
        title="increase fire"
        @left-button="decreaseFire()"
        @right-button="increaseFire()"
      > 
        <template #leftButton>-</template>
        <template #rightButton>+</template>
      </ControlButtons>

      <ControlButtons
        title="wind direction"
        @left-button="isLeftWind = true"
        @right-button="isLeftWind = false"
      > 
        <template #leftButton>&lt;</template>
        <template #rightButton>></template>
      </ControlButtons>

      <button class="matrix" :class="matrixMode ? 'blue' : 'red'" @click="matrixMode = !matrixMode">
        {{ `Take the ${matrixMode ? 'blue' : 'red' } pill` }}
        
        <img v-if="matrixMode" src="./assets/bluepill.png">
        <img v-else src="./assets/redpill.png">
      </button>
    </div>
  </main>
</template>

<script setup lang="ts">
  import { ref, onMounted } from 'vue';
  import FireBox from './components/FireBox.vue';
  import ControlButtons from './components/ControlButtons.vue';

  const firePixelArray = ref<number[]>([])
  const fireWidth = ref(60)
  const fireHeight = ref(60)
  const initialFireIntensity = ref(36)
  const matrixMode = ref(false)
  const isLeftWind = ref(true)

  const createFireDataScructure = () => sizedArray(fireHeight.value * fireWidth.value).forEach((_, ind) => firePixelArray.value[ind] = 0)

  const sizedArray = (range: number) => Array.from({ length: range })

  const setInitialValueOfFirePixels = () => {
    sizedArray(fireWidth.value).forEach((_, column) => {
      const overflowPixelIndex = fireHeight.value * fireWidth.value
      const pixelIndex = (overflowPixelIndex - fireWidth.value) + column
      
      firePixelArray.value[pixelIndex] = initialFireIntensity.value
    })
  }

  const calculateFirePropagation = () => {
    sizedArray(fireWidth.value).forEach((_, column) => {
      sizedArray(fireHeight.value).forEach((_, row) => {
        const pixelIndex = column + (fireWidth.value * row)

        updateFireIntesity(pixelIndex);
      })
    })
  }

  const updateFireIntesity = (currentPixelIndex: number) => {
    const belowPixelIndex = currentPixelIndex + fireWidth.value

    if (belowPixelIndex >= fireWidth.value * fireHeight.value) return

    const decay = Math.floor(Math.random() * 3)
    const belowPixelFireIntesity = firePixelArray.value[belowPixelIndex]
    const newFireIntesity = belowPixelFireIntesity - decay >= 0
      ? belowPixelFireIntesity - decay
      : 0

    firePixelArray.value[
      isLeftWind.value ? currentPixelIndex - decay : currentPixelIndex + decay
    ] = newFireIntesity
  }

  const increaseFire = () => {
    sizedArray(fireWidth.value).forEach((_, column) => {
      const overflowPixelIndex = fireWidth.value * fireHeight.value
      const pixelIndex = (overflowPixelIndex - fireWidth.value) + column
      const currentFireIntensity = firePixelArray.value[pixelIndex]

      if (currentFireIntensity < 36) {
        const increase = Math.floor(Math.random() * 14)
        const newFireIntensity = currentFireIntensity + increase >= 36
          ? 36
          : currentFireIntensity + increase

        firePixelArray.value[pixelIndex] = newFireIntensity
      }
    })
  }

  const decreaseFire = () => {
    sizedArray(fireWidth.value).forEach((_, column) => {
      const overflowPixelIndex = fireWidth.value * fireHeight.value
      const pixelIndex = (overflowPixelIndex - fireWidth.value) + column
      const currentFireIntensity = firePixelArray.value[pixelIndex]

      if (currentFireIntensity > 0) {
        const decay = Math.floor(Math.random() * 14)
        const newFireIntensity = currentFireIntensity - decay >= 0
          ? currentFireIntensity - decay
          : 0

        firePixelArray.value[pixelIndex] = newFireIntensity
      }
    })
  }

  onMounted(() => {
    createFireDataScructure()
    setInitialValueOfFirePixels()

    setInterval(calculateFirePropagation, 50)
  })
</script>

<style lang="scss">
@font-face {
  font-family: "Matrix";
  src: local("Matrix"), url(./assets/MatrixDotsDemoRegular.ttf) format("truetype");
}

body {
  background-color: rgb(35, 35, 35);
  margin: 0;
}

main {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  height: 100vh;

  button {
    &:hover {
      cursor: pointer;
    }
  }

  .buttons-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    margin-top: 2rem;

    button.matrix {
      background-color: transparent;
      border: none;
      
      font-weight: 600;
      font-size: 1rem;
      font-family: "Matrix";
  
      &.red {
        color: #D11022;
      }
  
      &.blue {
        color: #1B59C8;
      }
  
      img {
        height: 1rem;
      }
    }
  }
}
</style>
