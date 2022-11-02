<script setup>

</script>

<template>
  <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button 
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
    </div>

    <div class="border rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">
      
      <div 
        v-for="n in 16"
        class="flip-card">
        <div class="flip-card-inner">
          <div class="flip-card-front"></div>
          <div class="flip-card-back">
            <img :src="`/img/cat-01.jpg`" alt="">
          </div>
        </div>
      </div>

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