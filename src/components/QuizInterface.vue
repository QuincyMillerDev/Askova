<template>
  <div class="w-3/5 flex flex-col bg-white">
    <!-- Chat header with progress -->
    <ProgressBar
        :current-question="currentQuestion"
        :total-questions="totalQuestions"
        :quiz-started="quizStarted"
    />

    <!-- Chat messages -->
    <ChatMessages :messages="messages" :quiz-started="quizStarted" />

    <!-- Chat input -->
    <ChatInput
        :quiz-started="quizStarted"
        :waiting-for-next-question="waitingForNextQuestion"
        @send-message="handleSendMessage"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, nextTick } from "vue";
import ProgressBar from "./ProgressBar.vue";
import ChatMessages from "./ChatMessages.vue";
import ChatInput from "./ChatInput.vue";

const props = defineProps<{
  notes: string;
  quizStarted: boolean;
}>();

const emit = defineEmits<{
  (e: "quiz-completed"): void;
}>();

const messages = ref<Array<{ sender: "ai" | "user"; content: string }>>([]);
const currentQuestion = ref(0);
const totalQuestions = ref(5);
const waitingForNextQuestion = ref(false);

const askQuestion = () => {
  currentQuestion.value++;
  waitingForNextQuestion.value = false;

  // In a real app, you would generate questions based on the notes
  // For this demo, we'll use placeholder questions
  const questions = [
    "What is the main concept described in your notes?",
    "Explain the relationship between the key terms mentioned in your study material.",
    "What are the primary factors that influence the process described in your notes?",
    "Based on your notes, what would be the outcome if the conditions were changed?",
    "How would you apply the concepts from your notes to solve a real-world problem?",
  ];

  const questionIndex = Math.min(currentQuestion.value - 1, questions.length - 1);

  messages.value.push({
    sender: "ai",
    content: `Question ${currentQuestion.value}: ${questions[questionIndex]}`,
  });
};

const generateFeedback = (userResponse: string) => {
  // In a real app, you would compare the answer with the content from notes
  // For this demo, we'll provide generic feedback based on answer length
  if (userResponse.length < 20) {
    return "Your answer is quite brief. Consider expanding on your explanation and including more details from your study material.";
  } else if (userResponse.length < 50) {
    return "Good attempt! Your answer covers some key points, but you could include more specific examples from your notes to strengthen your response.";
  } else {
    return "Excellent answer! You've demonstrated a thorough understanding of the material. Your explanation is comprehensive and well-articulated.";
  }
};

const handleSendMessage = (message: string) => {
  if (!message.trim() || waitingForNextQuestion.value) return;

  // Add user message
  messages.value.push({
    sender: "user",
    content: message,
  });

  // Process response and provide feedback
  setTimeout(() => {
    // In a real app, you would evaluate the answer based on the notes
    // For this demo, we'll provide generic feedback
    const feedback = generateFeedback(message);
    messages.value.push({
      sender: "ai",
      content: feedback,
    });

    // Check if we should continue or end the quiz
    if (currentQuestion.value < totalQuestions.value) {
      waitingForNextQuestion.value = true;

      setTimeout(() => {
        messages.value.push({
          sender: "ai",
          content: "Are you ready for the next question? Type anything to continue.",
        });

        // Set up listener for next question
        waitingForNextQuestion.value = false;
      }, 1500);
    } else {
      // End of quiz
      setTimeout(() => {
        messages.value.push({
          sender: "ai",
          content: "Congratulations! You've completed the quiz. Would you like to start a new quiz with different material?",
        });

        emit("quiz-completed");
      }, 1500);
    }
  }, 1000);
};

const startQuiz = () => {
  currentQuestion.value = 0;
  messages.value = [];

  // Welcome message
  messages.value.push({
    sender: "ai",
    content: "Welcome to your personalized quiz! I'll ask you questions based on your notes. Let's begin!",
  });

  // Ask the first question
  nextTick(() => {
    askQuestion();
  });
};

// Expose methods to parent
defineExpose({
  startQuiz,
});
</script>
