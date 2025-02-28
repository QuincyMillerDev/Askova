<template>
  <div class="flex-grow overflow-y-auto p-4" ref="chatContainer">
    <div v-if="!quizStarted" class="flex items-center justify-center h-full">
      <div class="text-center text-gray-500">
        <IconBrain class="w-16 h-16 mx-auto mb-4 text-gray-300" />
        <p>Add your study material and start the quiz to begin</p>
      </div>
    </div>

    <div v-else>
      <div v-for="(message, index) in messages" :key="index" class="mb-4">
        <div
            :class="[
            'p-4 rounded-lg max-w-[85%]',
            message.sender === 'ai'
              ? 'bg-gray-100 mr-auto'
              : 'bg-blue-500 text-white ml-auto'
          ]"
        >
          {{ message.content }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, nextTick } from "vue";
import { Brain as IconBrain } from "lucide-vue-next";

const props = defineProps<{
  messages: Array<{ sender: "ai" | "user"; content: string }>;
  quizStarted: boolean;
}>();

const chatContainer = ref<HTMLElement | null>(null);

const scrollToBottom = () => {
  nextTick(() => {
    if (chatContainer.value) {
      chatContainer.value.scrollTop = chatContainer.value.scrollHeight;
    }
  });
};

// Watch for changes in messages to scroll to bottom
watch(
    () => props.messages.length,
    () => {
      scrollToBottom();
    }
);
</script>
