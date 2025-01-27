<template>
  <div :class="vertical && 'rotate-90'" class="bg-gray-500 h-full w-20">
    <hr class=" absolute w-0.5 border-dashed border-l -translate-x-1/2 left-1/2 h-full"></hr>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  vertical?: boolean;
}>();
</script>
