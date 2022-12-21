<template>
    <div class="flip-card" :class="{ 'open': isOpen }">
        <div class="flip-card-inner" v-if="!isShow">
            <div class="flip-card-front">{{ cardValue }}</div>
            <div class="flip-card-back">
                <img :src="`./img/cat-0${cardValue}.jpg`" alt="">
            </div>
        </div>
    </div>

</template>

<script setup>

const props = defineProps({
    key: String,
    cardValue: Number,
    isOpen: Boolean,
    isShow: Boolean,
});

</script>

<style scoped>
.flip-card {
  display: block;
  width: 150px;
  height: 100px;
  border: 1px solid #f1f1f1;
  perspective: 1000px;
  margin: 1rem;
}

.flip-card.open .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;
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