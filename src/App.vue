<template>
  <div class="h-screen w-screen bg-gray-100 flex overflow-hidden">
    <StudyMaterialInput
        :can-start-quiz="canStartQuiz"
        @notes-updated="updateNotes"
        @file-selected="updateSelectedFile"
        @start-quiz="startQuiz"
    />

    <QuizInterface
        ref="quizInterface"
        :notes="notes"
        :quiz-started="quizStarted"
        @quiz-completed="handleQuizCompleted"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import StudyMaterialInput from './components/StudyMaterialInput.vue';
import QuizInterface from './components/QuizInterface.vue';

// State
const notes = ref('');
const selectedFile = ref<File | null>(null);
const quizStarted = ref(false);
const quizInterface = ref<InstanceType<typeof QuizInterface> | null>(null);

// Computed properties
const canStartQuiz = computed(() => {
  return notes.value.trim().length > 0 || selectedFile.value !== null;
});

// Methods
const updateNotes = (newNotes: string) => {
  notes.value = newNotes;
};

const updateSelectedFile = (file: File) => {
  selectedFile.value = file;
};

const startQuiz = () => {
  if (!canStartQuiz.value) return;

  quizStarted.value = true;

  // Call the startQuiz method on the QuizInterface component
  if (quizInterface.value) {
    quizInterface.value.startQuiz();
  }
};

const handleQuizCompleted = () => {
  // You could add additional logic here if needed
  // For example, showing a summary or offering to restart
};
</script>
