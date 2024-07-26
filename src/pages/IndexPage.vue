<template>
  <div class="column">
    <h3 class="q-mt-sm q-mb-sm text-weight-regular text-center">
      Sorting Algorithm Visualizer
    </h3>
    <div class="q-px-xl">
      <q-badge> No.Of Bars: {{ noOfBars }} </q-badge>
      <q-slider
        v-model="noOfBars"
        :min="10"
        :max="400"
        :step="10"
        @update:model-value="
          arrayGenerator();
          arrayShuffler();
        "
      />
    </div>
    <div class="row q-gutter-md justify-center">
      <q-btn
        push
        color="primary"
        label="Selection Sort"
        @click="selectionSort(numbersArray)"
      />
      <q-btn
        push
        color="primary"
        label="Bubble Sort"
        @click="bubbleSort(numbersArray)"
      />
      <q-btn
        push
        color="primary"
        label="Insertion Sort"
        @click="insertionSort(numbersArray)"
      />
      <q-btn
        push
        color="primary"
        label="Shuffle"
        @click="
          arrayGenerator();
          arrayShuffler();
        "
      />
    </div>
  </div>
  <div class="container fixed-bottom">
    <div
      class="bar"
      v-for="(number, index) in numbersArray"
      :key="index"
      :style="{
        height: number * ((1.4 * 400) / noOfBars) + 'px',
        width: (2.5 * 400) / noOfBars + 'px',
        'margin-right': (0.5 * 400) / noOfBars + 'px',
      }"
    ></div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    const numbersArray = ref([]);
    const noOfBars = ref(100);

    function arrayGenerator() {
      console.log("arrayGenerator");
      numbersArray.value = [];
      for (var i = 1; i <= noOfBars.value; i++) {
        numbersArray.value.push(i);
      }
    }

    function arrayShuffler() {
      console.log("arrayShuffler");
      let currentIndex = numbersArray.value.length;

      while (currentIndex !== 0) {
        const randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        swapPositions(numbersArray.value, currentIndex, randomIndex);
      }
    }

    function swapPositions(array, a, b) {
      var temp = array[a];
      array[a] = array[b];
      array[b] = temp;
    }

    function sleep(n) {
      return new Promise((resolve) =>
        setTimeout(resolve, Math.floor((n * 400) / noOfBars.value))
      );
    }

    async function bubbleSort(arr) {
      var i, j;
      const n = arr.length;

      for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - i - 1; j++) {
          if (arr[j] > arr[j + 1]) {
            await sleep(0.5);
            swapPositions(arr, j, j + 1);
          }
        }
      }
    }

    async function insertionSort(arr) {
      var i, j, key;
      const n = arr.length;
      for (i = 1; i < n; i++) {
        key = arr[i];
        j = i - 1;
        while (j >= 0 && arr[j] > key) {
          await sleep(0.5);
          arr[j + 1] = arr[j];
          j = j - 1;
        }
        arr[j + 1] = key;
      }
    }

    async function selectionSort(arr) {
      var i, j, min_idx;
      const n = arr.length;
      for (i = 0; i < n - 1; i++) {
        min_idx = i;
        for (j = i + 1; j < n; j++) {
          if (arr[j] < arr[min_idx]) {
            await sleep(20);
            min_idx = j;
          }
        }
        swapPositions(arr, min_idx, i);
      }
    }

    arrayGenerator();
    arrayShuffler();

    return {
      numbersArray,
      arrayGenerator,
      arrayShuffler,
      swapPositions,
      bubbleSort,
      insertionSort,
      selectionSort,
      sleep,
      noOfBars,
    };
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: baseline;
}
.bar {
  background-color: black;
  display: inline-block;
  margin: 0 0.5px;
}
.fixed-bottom {
  bottom: 10px;
}
</style>