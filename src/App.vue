<script setup>
  import { ref, watch } from 'vue';
  import flipCard from './components/flipCard.vue';

  const cards = ref([]);
  const openedCard = ref([]);
  
  const gameInit = () => {
    const numArr = [...new Array(16).keys()].map(i => ++i);
    numArr.sort(() => Math.random() - 0.5);
    cards.value = numArr.map(d => (d % 8) + 1);
    openedCard.value = [];
  }

  const clickHandler = (idx) => {    
    const i = openedCard.value.indexOf(idx);
    const n = cards.value[idx];

    if (openedCard.value.length < 2) {
      if(i >= 0) {
        openedCard.value.splice(i, 1);
      }
      else {
        openedCard.value.push(idx);
      }
    }

    // card match
    if (openedCard.value.length === 2) {
      if(cards.value[openedCard.value[0]] === cards.value[openedCard.value[1]]) {
        window.setTimeout(() => {
          cards.value = cards.value.map(d => (d === n) ? -1 : d);
          openedCard.value = [];
        }, 1000);
      }
      else {
        window.setTimeout(() => {
          openedCard.value = [];
        }, 1000);
      }
    }
  }

  watch(cards, v => {
    if(v.filter(d => d > 0).length === 0) {
      confirm('恭喜破關，再來一局？') && gameInit();
    }
  });
</script>

<template>
  <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button 
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">{{ cards.length > 0 ? '重置' : '開始' }}</button>
    </div>

    <div class="border rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">

      <flipCard 
        v-for="(n, idx) in cards" 
        :isOpened="openedCard.includes(idx)"
        :isMatch="cards[idx] === -1"
        :cardIdx="n"
        @click="clickHandler(idx)" />
    </div>
  </div>
</template>