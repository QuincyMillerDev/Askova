<template>
  <div class="p-4 border-t border-gray-200">
    <div class="grid grid-cols-12 gap-2">
      <div class="col-span-10">
        <input
            type="text"
            v-model="inputText"
            @keyup.enter="sendMessage"
            class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 text-gray-900 bg-white placeholder-gray-500"
            placeholder="Type your answer here..."
            :disabled="!quizStarted || waitingForNextQuestion"
        />
      </div>
      <div class="col-span-2">
        <button
            @click="sendMessage"
            class="w-full h-full px-4 py-3 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition-colors flex items-center justify-center"
            :disabled="!quizStarted || waitingForNextQuestion"
        >
          <IconSend class="w-5 h-5" />
        </button>
      </div>
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
