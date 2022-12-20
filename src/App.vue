<script setup>
// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開
//  2. 點擊兩張不同的卡片，卡片會翻回去
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

import { ref } from 'vue';
import cardVue from './card.vue';

const cards = ref([]);
const openedCard = ref([]);
const twoCards = ref([]);
const hideCards = ref([]);

const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
  hideCards.value = [];
}

const clickHandler = (idx) => {
  if (twoCards.value.length + 1 <= 2) {
    openedCard.value.push(idx);
    twoCards.value.push(cards.value[idx]);
    if (twoCards.value.length > 1) {
      if (twoCards.value[0] != twoCards.value[1]) {
        // 若翻牌value不相等
        window.setTimeout(() => {
          // 移除最後兩個元素
          openedCard.value.pop();
          openedCard.value.pop();
          twoCards.value = [];
        }, 1000);
      }
      else {
        // 若翻牌value相等, 則加入卡片消失array
        window.setTimeout(() => {
          hideCards.value.push(cards.value[idx]);
        }, 1000);
        twoCards.value = [];
      }
    }
  }
  else {
    twoCards.value = [];
  }

  if (openedCard.value.length == cards.value.length) {
    window.setTimeout(() => {
      if (confirm('恭喜破關，再來一局？')) {
        gameInit();
      }
    }, 1000);
  }
}
</script>

<template>
  <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
    </div>

    <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">

      <cardVue
        v-for="(n, idx) in cards"
        :key="idx" :cardValue="n"
        :isOpen="openedCard.includes(idx)"
        :isShow="hideCards.includes(n) > 0 && hideCards.length > 0"
        @click="clickHandler(idx)"
      />

    </div>
  </div>
</template>

<style scoped>

</style>