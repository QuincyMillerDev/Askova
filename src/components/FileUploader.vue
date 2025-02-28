<template>
  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1">Or upload a file</label>
    <div
        class="border-2 border-dashed border-gray-300 rounded-lg p-6 text-center cursor-pointer hover:bg-gray-50 transition-colors"
        @click="triggerFileInput"
        @dragover.prevent
        @drop.prevent="handleFileDrop"
    >
      <input
          type="file"
          ref="fileInput"
          class="hidden"
          @change="handleFileChange"
          accept=".txt,.pdf,.doc,.docx"
      />
      <div class="flex flex-col items-center">
        <IconUpload class="w-10 h-10 text-gray-400 mb-2" />
        <p class="text-sm text-gray-500">
          Drag and drop your file here, or click to browse
        </p>
        <p v-if="selectedFile" class="mt-2 text-sm text-green-600 font-medium">
          {{ selectedFile.name }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { Upload as IconUpload } from "lucide-vue-next";

const selectedFile = ref<File | null>(null);
const fileInput = ref<HTMLInputElement | null>(null);

const emit = defineEmits<{
  (e: "file-selected", file: File): void;
  (e: "content-updated", content: string): void;
}>();

const triggerFileInput = () => {
  if (fileInput.value) {
    fileInput.value.click();
  }
};

const handleFileChange = (event: Event) => {
  const target = event.target as HTMLInputElement;
  if (target.files && target.files.length > 0) {
    selectedFile.value = target.files[0];
    emit("file-selected", selectedFile.value);

    // In a real app, you would parse the file content here
    // For this demo, we'll just use the file name
    const fileContent = `Content from file: ${selectedFile.value.name}`;
    emit("content-updated", fileContent);
  }
};

const handleFileDrop = (event: DragEvent) => {
  if (event.dataTransfer?.files && event.dataTransfer.files.length > 0) {
    selectedFile.value = event.dataTransfer.files[0];
    emit("file-selected", selectedFile.value);

    // In a real app, you would parse the file content here
    const fileContent = `Content from file: ${selectedFile.value.name}`;
    emit("content-updated", fileContent);
  }
};
</script>
