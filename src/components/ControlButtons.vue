<template>
    <div class="increase-buttons-wrapper">
        <button @click="emit('leftButton')">
            <slot name="leftButton"></slot>
        </button>

        <span>{{ title }}</span>
        
        <button @click="emit('rightButton')">
            <slot name="rightButton"></slot>
        </button>
      </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

const emit = defineEmits(['leftButton', 'rightButton'])

defineProps({
    title: {
        type: String,
        required: true
    },
})
</script>

<style lang="scss">
.increase-buttons-wrapper {
    display: flex;
    align-items: center;

    span {
        font-family: "Matrix";
        color: white;
        font-size: 1rem;
        padding-bottom: 1.7rem;
    }
    
    button {
        background-color: transparent;
        border: none;
        font-size: 2rem;
        color: white;
        padding: 0 2rem 2rem 2rem;
    }
}
</style>
  