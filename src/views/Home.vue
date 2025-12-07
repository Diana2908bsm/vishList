<script setup lang="ts">
import { onMounted, ref, computed } from 'vue';
import type { IGifts } from '../types/index'
import Gifts from '@/components/gifts.vue';

const gifts = ref<IGifts[]>([])


async function getGifts() {
    try {
        const response = await fetch('http://194.58.114.16/api/wishlistitems')
        const data = await response.json()
        gifts.value = data

    } catch (err) {
        console.log(err)
    }
}
const toggleButton = async (gift: IGifts) => {
    const oldStatus = gift.isReserved
    const newStatus = !gift.isReserved;
    gift.isReserved = newStatus;
    try {

        await fetch('http://194.58.114.16/api/wishlistitems', {
            method: "PUT",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({
                id: gift.id,
                isReserved: newStatus
            })
        })
    } catch (err) {
        console.log(err)
        gift.isReserved = oldStatus

    }

}
const generalGifts = computed(() => gifts.value.filter(g => g.belongsTo === 0))
const dianaGifts = computed(() => gifts.value.filter(g => g.belongsTo === 1))
const daniilGifts = computed(() => gifts.value.filter(g => g.belongsTo === 2))
onMounted(async () => {
    await getGifts()
})
</script>


<template>
    <header class="header">
        <div class="header__container container">
            <div class="header__vishlist">Вишлист</div>
            <h1 class="header__title">New Year 🎄</h1>
            <p class="header__text">Это наш общий вишлист. Здесь собраны как подарки для нас обоих, так и
                индивидуальные пожелания каждого</p>
        </div>
    </header>

    <div class="main container">
        <div class="main__box">
            <h2 class="title">Общее</h2>
            <div class="main__inner">
                <div v-for="item in generalGifts" :key="item.id">
                    <Gifts :gift="item" @toggleButton="toggleButton" />
                </div>
            </div>
        </div>
        <div class="main__box">
            <h2 class="title">Диана</h2>
            <div class="main__inner">
                <div v-for="item in dianaGifts" :key="item.id">
                    <Gifts :gift="item" @toggleButton="toggleButton" />
                </div>
            </div>
        </div>
        <div class="main__box">
            <h2 class="title">Даня</h2>
            <div class="main__inner">
                <div v-for="item in daniilGifts" :key="item.id">
                    <Gifts :gift="item" @toggleButton="toggleButton" />
                </div>
            </div>
        </div>
    </div>
</template>
