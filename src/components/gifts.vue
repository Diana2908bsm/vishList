<script setup lang="ts">
import type { IGifts } from '../types/index'
const props = defineProps<{
    gift: IGifts
}>()
const emit = defineEmits(['toggleButton'])
const reservedButton = ()=>{
   emit('toggleButton', props.gift)
}
</script>
<template>
    <div class="gifts">
        <div class="gifts__box">
            <div class="gifts__img">
                <img :src="`../assets/${gift.imageUrl}`" :alt="gift.name" />
            </div>
            <div class="gifts__info">
                <div class="gifts__title">{{ gift.name }}</div>
                <div class="gifts__price">{{ gift.price }} ₽</div>
            </div>
        </div>
        <div class="gifts__buttons">
            <a class="gifts__link" :href="gift.link" target="_blank">
                Где купить
                <svg xmlns="http://www.w3.org/2000/svg" width="8" height="8" viewBox="0 0 8 8" fill="none">
                    <path
                        d="M6.20895 2.11517L0.876214 7.44791L-1.71386e-05 6.57168L5.33272 1.23894L0.632621 1.23894L0.632622 -5.31858e-05L7.44795 -5.28486e-05L7.44795 6.81527L6.20896 6.81527L6.20895 2.11517Z"
                        fill="black" />
                </svg>
            </a>

            <button @click="reservedButton" :class="['gifts__button', { 'reserved-button': gift.isReserved }]">{{ gift.isReserved ? 'Забронировано' :'Забронировать' }}</button>
        </div>
    </div>

</template>
