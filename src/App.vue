<script setup>
// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開
//  2. 點擊兩張不同的卡片，卡片會翻回去
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

import { ref } from 'vue';
import myCard from './myCard.vue'

const cards = ref([]);
const openedCard = ref([]);
const msg = ref('快來挑戰');

const gameInit = () => {
    const numArr = [...new Array(16).keys()].map(i => ++i);
    numArr.sort(() => Math.random() - 0.5);
    cards.value = numArr.map(d => (d % 8) + 1);
    openedCard.value = [];
    msg.value = '任務開始';
}

const restartGame = () => {
    const res = window.confirm('Mission Completed,  Restart A New One?')
    if (res) {
        gameInit();
    }else{
      msg.value = 'Bye';
    }
}

const clickHandler = (idx) => {
    // console.log(idx)
    const cardNum = cards.value[idx];
    // 防止連點同一圖
    if (openedCard.value.length < 2) {
        if (!openedCard.value.includes(idx)) {
            openedCard.value.push(idx);
        }
    }
    // 比對兩圖
    if (openedCard.value.length === 2) {
        if (cards.value[openedCard.value[0]] === cards.value[openedCard.value[1]]) {
            msg.value = '答對了～';
            window.setTimeout(() => {
                cards.value = cards.value.map((e) => e === cardNum ? null : e);
                if (cards.value.some((e) => e !== null)) {
                } else {
                    msg.value = '任務完成！';
                    restartGame();
                }
                openedCard.value = [];
            }, 2000);
        } else {
            msg.value = 'Oooops...'
            window.setTimeout(() => {
                openedCard.value = [];
                msg.value = '...'
            }, 2000);
        }

    } else {
        window.setTimeout(() => {
            openedCard.value = [];
            msg.value = '加油'
        }, 2000);
    }
// Q: 為何卡片會先翻回去才消失？
}
</script>
<template>
    <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">
        <div class="my-10 text-white text-center ">
            <div class="mb-8 text-5xl">五倍對對碰</div>
            <button @click="gameInit" class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
            <p class="mt-5">{{msg}}</p>
        </div>
        <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">
            <my-card 
              v-for="(n, idx) in cards" 
              :pn="n" 
              :pIdx="idx" 
              :pCards="cards" 
              :pOpenedCard="openedCard" @clickCard="clickHandler(idx)" />
            <!-- <div 
                v-for="(n, idx) in cards" 
                class="flip-card" 
                :class="{'open': openedCard.includes(idx) }" 
                @click="clickHandler(idx)">
            <div class="flip-card-inner" v-if="cards[idx] > 0">
                <div class="flip-card-front">{{ n }}</div>
                <div class="flip-card-back">
                    <img :src="`./img/cat-0${n}.jpg`" alt="">
                </div>
            </div>
        </div> -->
        </div>
        <p class="text-white">cards: {{cards}}</p>
        <p class="text-white">openedCard: {{openedCard}}</p>
    </div>
</template>
<style scoped>
</style>