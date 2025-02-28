<template>
  <div class="w-2/5 flex flex-col p-8 overflow-y-auto border-r border-gray-200">
    <h2 class="text-4xl font-bold mb-4">
      <span class="text-blue-500">Learn by testing</span>
    </h2>
    <p class="text-lg text-gray-700 mb-6">
      Upload your notes or paste content, and our AI will generate quiz questions to test your knowledge.
      Master concepts through active recall.
    </p>

    <div class="bg-white rounded-lg shadow-md p-6 mb-6">
      <h3 class="text-lg font-semibold mb-3">Add your study material</h3>

      <div class="mb-4">
        <label for="notes" class="block text-sm font-medium text-gray-700 mb-1">Paste your notes</label>
        <textarea
            id="notes"
            v-model="notes"
            class="w-full h-40 p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 text-gray-900 bg-white"
            placeholder="Paste your study notes here..."
            @input="$emit('notes-updated', notes)"
        ></textarea>
      </div>

      <FileUploader
          @file-selected="handleFileSelected"
          @content-updated="handleContentUpdated"
      />

      <button
          @click="$emit('start-quiz')"
          class="w-full py-3 px-6 bg-green-500 hover:bg-green-600 text-white font-medium rounded-lg transition-colors mt-6"
          :disabled="!canStartQuiz"
          :class="{'opacity-50 cursor-not-allowed': !canStartQuiz}"
      >
        Start Quiz
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import FileUploader from "./FileUploader.vue";

const props = defineProps<{
  canStartQuiz: boolean;
}>();

const emit = defineEmits<{
  (e: "start-quiz"): void;
  (e: "notes-updated", notes: string): void;
  (e: "file-selected", file: File): void;
}>();

const notes = ref("");
const selectedFile = ref<File | null>(null);

const handleFileSelected = (file: File) => {
  selectedFile.value = file;
  emit("file-selected", file);
};

const handleContentUpdated = (content: string) => {
  notes.value = content;
  emit("notes-updated", content);
};
</script>
