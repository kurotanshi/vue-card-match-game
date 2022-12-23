<script setup>
// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開
//  2. 點擊兩張不同的卡片，卡片會翻回去
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

import { ref } from 'vue';
import Card from './card.vue'
import CustomModal from './customModal.vue'

const cardNumbers = ref([]);
const isModalOpen = ref(false);
const openedCard = ref([]);
const pickedCard = ref([]);

const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cardNumbers.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
  pickedCard.value = [];
  isModalOpen.value = false
}

const clickHandler = (idx) => {
  if (openedCard.value.includes(idx)) return;
  if (pickedCard.value.includes(idx)) return;
  if (openedCard.value.length > 1) return;

  openedCard.value.push(idx);
  if (openedCard.value.length === 2) checkCardNumbers(openedCard.value[0], openedCard.value[1]);
}

const checkCardNumbers = (idx1, idx2) => {
  window.setTimeout(() => {
    if (cardNumbers.value[idx1] === cardNumbers.value[idx2]) {
      pickedCard.value.push(idx1);
      pickedCard.value.push(idx2);
    }

    openedCard.value = [];

    if (pickedCard.value.length === 16) isModalOpen.value = true
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
        :isPicked="pickedCard.includes(idx)"
        :imgUrl="`./img/cat-0${cardNumber}.jpg`"
        :number="cardNumber"
        @click="clickHandler(idx)"
      />
    </div>
  </div>

  <CustomModal :open="isModalOpen" @restartGame="gameInit"/>
</template>

<style scoped>
</style>
