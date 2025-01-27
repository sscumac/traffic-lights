<template>
  <div class="rounded-md bg-gray-700 p-1 flex gap-1 max-w-fit" :class="vertical && '-rotate-90'">
    <div
      :class="state === 'green' && 'bg-green-700'"
      class="rounded-full w-5 h-5 bg-gray-300 transition-all duration-1000"
    />
    <div
      v-if="!isPedestrian"
      :class="(state === 'yellow' || state === 'yellow-red') && 'bg-yellow-400'"
      class="rounded-full w-5 h-5 bg-gray-300 transition-all duration-1000"
    />
    <div
      :class="(state === 'red' || state === 'yellow-red') && 'bg-red-600'"
      class="rounded-full w-5 h-5 bg-gray-300 transition-all duration-1000"
    />
  </div>
</template>

<script setup lang="ts">
defineProps<{
  state: string;
  isPedestrian?: boolean;
  vertical?: boolean;
}>();
</script>
