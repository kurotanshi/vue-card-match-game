<script setup>
// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開
//  2. 點擊兩張不同的卡片，卡片會翻回去
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

import { ref, computed } from 'vue';
import card from './Card.vue';

const cards = ref([]);
const openedCard = computed(() => cards.value.filter((n) => n.isAvailable && n.isOpened));

const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  //cards.value = numArr.map(d => (d % 8) + 1);
  cards.value = [];
  numArr.map(d => (d % 8) + 1).forEach((value, index) => {
    cards.value.push({
      idx: index,
      imageId: value,
      isOpened: false,
      isAvailable: true
    })
  });
  //openedCard.value = [];
}

const clickHandler = (idx) => {
  if (!cards.value[idx].isAvailable || (!cards.value[idx].isOpened && openedCard.value.length === 2)) return;

  cards.value[idx].isOpened = !cards.value[idx].isOpened;
  if (openedCard.value.length !== 2) return;

  window.setTimeout(() => {
    if (openedCard.value[0].imageId === openedCard.value[1].imageId)
    {
      openedCard.value.forEach((item) => {
        item.isAvailable = false;
      })
      if (cards.value.find(item => item.isAvailable) === undefined) {
        window.setTimeout(() => {
          if (confirm('恭喜破關，再來一局？')) {
            gameInit();
          }
        });
      }
    }
    else
    {
      openedCard.value.forEach((item) => {
        item.isOpened = false;
      })
    }
  }, 1000);
  // openedCard.value.push(idx);
  
  // window.setTimeout(() => {
  //   openedCard.value = [];
  // }, 1000);
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

      <card v-for="(n, idx) in cards" v-bind="n" @click="clickHandler(idx)" />

      <!-- <div 
        v-for="(n, idx) in cards"
        class="flip-card"
        :class="{
          'open': openedCard.includes(idx)
        }"
        @click="clickHandler(idx)">
        <div class="flip-card-inner" v-if="cards[idx] > 0">
          <div class="flip-card-front"></div>
          <div class="flip-card-back">
            <img :src="`./img/cat-0${n}.jpg`" alt="">
          </div>
        </div>
      </div> -->
    </div>
  </div>
</template>

<style scoped>
.flip-card {
  display: block;
  width: 150px;
  height: 100px;
  border: 1px solid #f1f1f1;
  perspective: 1000px;
  margin: 1rem;
}

.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

.flip-card.open .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-front,
.flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 5px solid #aaa;
  -webkit-backface-visibility: hidden;
  /* Safari */
  backface-visibility: hidden;
}

.flip-card-front {
  background-color: #eee;
  background-image: url('./img/cat-bg.png');
  background-size: contain;
  background-repeat: no-repeat;
  background-position-x: 47.5%;
  color: black;
}

.flip-card-back {      
  background-color: #eee;
  transform: rotateY(180deg);
}

.flip-card-back img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: contain;
  background-color: #eee;
}
</style>