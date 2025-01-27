<template>
  <div class="container py-20 mx-auto">
    <div class="mx-auto w-[640px] h-[640px]">
      <h1 class="text-xl font-semibold my-10">My Traffic Lights Controls</h1>

      <div ref="map" class="relative w-full h-full bg-gray-50 border-black mx-auto">
        <Street class="absolute left-1/2 -translate-x-1/2" />
        <Street :vertical="true" class="absolute left-1/2 -translate-x-1/2" />
        <div class="bg-gray-500 w-20 h-20 absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2" />
        <div class="absolute top-1/2 left-[74.5%] -translate-y-1/2 w-20 h-20 flex flex-col justify-between py-1.5">
          <div class="bg-white w-full h-1.5" v-for="(n, index) in Array.from({ length: 5 })" :key="index" />
        </div>

        <TrafficLights :state="stateMain" class="absolute top-[37%] left-[29%]" />
        <TrafficLights :state="stateSide" :vertical="true" class="absolute top-[62%] left-[54%]" />

        <div class="absolute top-[30%] flex flex-col items-center test-center left-[77%]">
          <TrafficLights :state="statePed" :is-pedestrian="true" :vertical="true" />
          <button
            class="rounded-full flex justify-center items-center w-6 h-6 mt-5 border-black border-2 hover:bg-blue-500 transition-all"
            @click="pedestrianActivated = true"
          >
            <span class="rounded-full bg-blue-500 w-2 h-2" />
          </button>
        </div>
      </div>

      <p class="mt-4">
        Press start to initiate cycle. Use the small button by the crosswalk to request green lights for pedestrians.
      </p>
      <div class="w-full text-center">
        <button
          class="mx-auto my-4 text-gray-900 border-2 border-gray-900 hover:border-gray-500 hover:text-gray-500 p-2 transition-all"
          @click="start"
        >
          Start
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import TrafficLights from "./TrafficLights.vue";
import Street from "./Street.vue";

const stateMain = ref("green");
const stateSide = ref("red");
const statePed = ref("red");

const pedestrianActivated = ref(false);

let cycleTimeouts: ReturnType<typeof setTimeout>[] = [];

function cycleTrafficLights() {
  cycleTimeouts = [];
  stateSide.value = "yellow-red";

  cycleTimeouts.push(setTimeout(() => (stateMain.value = "yellow"), 2000));
  cycleTimeouts.push(setTimeout(() => (stateSide.value = "green"), 2000));

  cycleTimeouts.push(setTimeout(() => (stateMain.value = "red"), 3000));

  cycleTimeouts.push(setTimeout(() => (stateMain.value = "yellow-red"), 5000));

  cycleTimeouts.push(setTimeout(() => (stateMain.value = "green"), 7000));
  cycleTimeouts.push(setTimeout(() => (stateSide.value = "yellow"), 7000));

  cycleTimeouts.push(setTimeout(() => (stateSide.value = "red"), 8000));

  cycleTimeouts.push(setTimeout(() => cycleTrafficLights(), 10000));
}

function cyclePedestrianLights() {
  setTimeout(() => (statePed.value = "green"), 1000);
  setTimeout(() => (statePed.value = "red"), 5000);

  if (stateMain.value !== "red") {
    stateMain.value = "yellow";
    setTimeout(() => (stateMain.value = "red"), 1000);
  }
  if (stateSide.value !== "red") {
    stateSide.value = "yellow";
    setTimeout(() => (stateSide.value = "red"), 1000);
  }
  pedestrianActivated.value = false;
  setTimeout(() => (stateMain.value = "yellow-red"), 4000);
  setTimeout(() => (stateMain.value = "green"), 7000);
  setTimeout(() => cycleTrafficLights(), 10000);
}

watch(pedestrianActivated, (newValue) => {
  if (newValue) {
    cycleTimeouts.forEach(clearTimeout);
    cycleTimeouts = [];
    cyclePedestrianLights();
  }
});

function start() {
  cycleTrafficLights();
}
</script>
