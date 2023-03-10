<template>
  <div class="main">
    <button @click="heavyIncrease">click</button>
    <button @click="reset" style="margin-left: 20px;">rest</button>
    <div style="color: #e33131">{{number1}}</div>
    <div style="color: #f5d743">{{number2}}</div>
    <div style="color: #41f85f">{{number3}}</div>
    <div style="color: #700de6">{{number4}}</div>

  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const number1 = ref(0);
const number2 = ref(0);
const number3 = ref(0);
const number4 = ref(0);
const reset = () => {
  number1.value = 0;
  number2.value = 0;
  number3.value = 0;
  number4.value = 0;
}

const heavyIncrease = () => {
  for (let i = 0; i < 10e8; i++) {
    if (i === 10e7) {
      number1.value = 1;
    }

    if (i === 10e7 * 3) {
      number2.value = 2;
    }

    if (i === 10e7 * 6) {
      number3.value = 3;
    }

    if (i === 10e7 * 9) {
      number4.value = 4;
    }
  }
}

</script>

<style>
  .main {
    margin: 0 20px;
  }

  .main div {
    margin: 10px 0;
  }
</style>
