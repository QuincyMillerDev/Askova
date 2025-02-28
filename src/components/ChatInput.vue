<template>
  <div class="p-4 border-t border-gray-200">
    <div class="flex">
      <input
          type="text"
          v-model="inputText"
          @keyup.enter="sendMessage"
          class="flex-grow p-3 border border-gray-300 rounded-l-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          placeholder="Type your answer here..."
          :disabled="!quizStarted || waitingForNextQuestion"
      />
      <button
          @click="sendMessage"
          class="px-4 py-3 bg-blue-500 text-white rounded-r-lg hover:bg-blue-600 transition-colors"
          :disabled="!quizStarted || waitingForNextQuestion"
      >
        <IconSend class="w-5 h-5" />
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { Send as IconSend } from "lucide-vue-next";

const props = defineProps<{
  quizStarted: boolean;
  waitingForNextQuestion: boolean;
}>();

const emit = defineEmits<{
  (e: "send-message", message: string): void;
}>();

const inputText = ref("");

const sendMessage = () => {
  if (!inputText.value.trim() || props.waitingForNextQuestion) return;

  emit("send-message", inputText.value);
  inputText.value = "";
};
</script>
