<template>
  <div class="p-4 border-b border-gray-200">
    <div class="flex justify-between items-center mb-1">
      <h3 class="font-medium">Quiz Progress</h3>
      <span class="text-green-600 font-medium">{{ progress }}</span>
    </div>
    <div class="w-full bg-gray-200 rounded-full h-2">
      <div
          class="bg-green-500 h-2 rounded-full"
          :style="`width: ${progressPercentage}%`"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  currentQuestion: number;
  totalQuestions: number;
  quizStarted: boolean;
}>();

const progressPercentage = computed(() => {
  if (!props.quizStarted) return 0;
  return Math.min(100, (props.currentQuestion / props.totalQuestions) * 100);
});

const progress = computed(() => {
  return `${props.currentQuestion}/${props.totalQuestions}`;
});
</script>
