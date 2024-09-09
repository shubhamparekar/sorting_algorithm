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
        :disable="disable"
        @update:model-value="
          arrayGenerator();
          arrayShuffler();
        "
      />
      <q-badge> Speed: {{ speed }}ms </q-badge>
      <q-slider
        v-model="speed"
        :min="10"
        :max="1000"
        :step="100"
        :disable="disable"
      />
    </div>
    <div class="row q-gutter-md justify-center">
      <q-btn
        push
        color="primary"
        label="Selection Sort"
        :disable="disable"
        @click="selectionSort(numbersArray)"
      />
      <q-btn
        push
        color="primary"
        label="Bubble Sort"
        :disable="disable"
        @click="bubbleSort(numbersArray)"
      />
      <q-btn
        push
        color="primary"
        label="Insertion Sort"
        :disable="disable"
        @click="insertionSort(numbersArray)"
      />
      <q-btn
        push
        color="primary"
        label="Shuffle"
        :disable="disable"
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
      :class="{
        selected: selectedIndices.includes(index),
        compared: comparedIndices.includes(index),
        smallest: smallestIndices.includes(index),
      }"
      :style="{
        height: number * ((1.25 * 400) / noOfBars) + 'px',
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
    const selectedIndices = ref([]);
    const comparedIndices = ref([]);
    const smallestIndices = ref([]);
    const speed = ref(510);
    const disable = ref(false);

    function arrayGenerator() {
      numbersArray.value = [];
      for (var i = 1; i <= noOfBars.value; i++) {
        numbersArray.value.push(i);
      }
    }

    function arrayShuffler() {
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

    function sleep() {
      return new Promise((resolve) => setTimeout(resolve, speed.value));
    }

    async function bubbleSort(arr) {
      const n = arr.length;
      disable.value = true;
      for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - i - 1; j++) {
          comparedIndices.value = [j, j + 1];
          if (arr[j] > arr[j + 1]) {
            await sleep();
            swapPositions(arr, j, j + 1);
          }
          await sleep();
        }
      }
      disable.value = false;
      comparedIndices.value = [];
    }

    async function insertionSort(arr) {
      const n = arr.length;
      disable.value = true;
      for (let i = 1; i < n; i++) {
        const key = arr[i];
        let j = i - 1;
        while (j >= 0 && arr[j] > key) {
          comparedIndices.value = [j, j + 1];
          await sleep();
          arr[j + 1] = arr[j];
          j = j - 1;
        }
        arr[j + 1] = key;
        await sleep();
        comparedIndices.value = [];
      }
      disable.value = false;
      comparedIndices.value = [];
    }

    async function selectionSort(arr) {
      const n = arr.length;
      disable.value = true;
      for (let i = 0; i < n - 1; i++) {
        let min_idx = i;
        selectedIndices.value = [i];
        for (let j = i + 1; j < n; j++) {
          await sleep();
          comparedIndices.value = [j];
          if (arr[j] < arr[min_idx]) {
            smallestIndices.value = [j];
            min_idx = j;
          }
        }
        swapPositions(arr, min_idx, i);
        await sleep();
      }
      disable.value = false;
      comparedIndices.value = [];
      smallestIndices.value = [];
      selectedIndices.value = [];
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
      selectedIndices,
      comparedIndices,
      smallestIndices,
      speed,
      disable,
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
.bar.selected {
  background-color: #ff4757;
}
.bar.compared {
  background-color: #1e90ff;
}
.bar.smallest {
  background-color: gold;
}
.fixed-bottom {
  bottom: 10px;
}
</style>