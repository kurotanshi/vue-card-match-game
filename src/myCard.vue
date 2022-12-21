<script setup>
import { ref } from 'vue';
const props = defineProps({
    pCards: Array,
    pOpenedCard: Array,
    pn: Number,
    pIdx: Number
})
const emit = defineEmits(['clickCard'])
</script>
<template>
    <!-- <div v-for="(n, idx) in cards" class="flip-card" :class="{
          'open': openedCard.includes(idx)
        }" @click="clickHandler(idx)"> -->
    <div class="flip-card" :class="{'open': pOpenedCard.includes(pIdx) }" @click="$emit('clickCard', pIdx)">
        <div class="flip-card-inner" v-if="pCards[pIdx] > 0">
            <div class="flip-card-front">{{ pn }}</div>
            <div class="flip-card-back">
                <img :src="`./img/cat-0${ pn }.jpg`" alt="">
            </div>
        </div>
    </div>
    <!-- </div> -->
</template>
<style>
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