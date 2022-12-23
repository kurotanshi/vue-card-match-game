<script setup>
// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開
//  2. 點擊兩張不同的卡片，卡片會翻回去
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

import { ref } from 'vue';
import Card from './card.vue'

const cardNumbers = ref([]);
const openedCard = ref([]);

const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cardNumbers.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
}

const clickHandler = (idx) => {    
  openedCard.value.push(idx);
  
  window.setTimeout(() => {
    openedCard.value = [];
  }, 1000);
}
</script>

<template>
  <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button 
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
    </div>

    <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">
      <Card
        v-for="(cardNumber, idx) in cardNumbers"
        :isOpen="openedCard.includes(idx)"
        :isPicked="false"
        :imgUrl="`./img/cat-0${cardNumber}.jpg`"
        @click="clickHandler(idx)"
      />
    </div>
  </div>
</template>

<style scoped>
</style>
