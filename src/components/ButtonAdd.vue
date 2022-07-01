<script setup>
import { onUnmounted } from 'vue';
import BaseButton from '/src/components/BaseButton.vue';

onUnmounted(() => {
  console.log('onUnmounted');
});
const emit = defineEmits(['add-click']);
</script>

<template>
  <BaseButton color="blue" @on-click="emit('add-click')">追加</BaseButton>
</template>
