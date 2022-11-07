<script setup>
const props = defineProps({
  isOpened: {
    type: Boolean,
    default: false
  },
  isMatch: {
    type: Boolean,
    default: false
  },
  cardIdx: {
    type: Number,
    default: 1
  },  
});
</script>

<template>
  <div 
    class="flip-card" :class="{ 'open': isOpened }">
    <div class="flip-card-inner" v-if="!isMatch">
      <div class="flip-card-front">{{cardIdx}}</div>
      <div class="flip-card-back">
        <img :src="`/img/cat-0${cardIdx}.jpg`" alt="">
      </div>
    </div>
  </div>
</template>

<style scoped>
.flip-card {
  @apply border border-[#eee] border-2 h-[100px] m-4 w-[150px] block;
}

.flip-card-inner {
  @apply h-full text-center w-full relative;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

.flip-card.open .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-front,
.flip-card-back {
  @apply border border-[#ddd] h-full border-2 w-full absolute;
  backface-visibility: hidden;
}

.flip-card-front {
  @apply bg-zinc-100;
  /* background-color: #eee; */
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