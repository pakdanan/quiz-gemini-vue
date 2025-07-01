<script setup>
import { GoogleGenAI, Type } from "@google/genai";
import StartScreen from './components/StartScreen.vue';
import Loader from "./components/Loader.vue";
import Quiz from "./components/Quiz.vue";
import Result from "./components/Result.vue";
import { ref } from 'vue';
const questions = ref('');
const status = ref('start'); // 'start', 'loading', 'ready', 'finished'
const userAnswers = ref([]);
const errorMessage = ref('');
const restartQuiz = () => {
  questions.value = '';
  status.value = 'start';
  userAnswers.value = [];
  errorMessage.value = '';
}
const storeAnswer = (answer) => {
  userAnswers.value.push(answer);
}
const startQuiz = async (topic) => {

  status.value = 'loading';
  const prompt = `
      Create 3 quiz questions about ${topic}
      Difficulty: Easy
      Type: Multiple Choice
    `;
  const schema = {
    description: "List of quiz questions",
    type: Type.OBJECT,
    properties: {
      response_code: {
        type: Type.NUMBER,
        description: "Response code indicating the status of the request",
        nullable: false,
      },
      results: {
        type: Type.ARRAY,
        description: "Array of quiz questions",
        items: {
          type: Type.OBJECT,
          properties: {
            type: {
              type: Type.STRING,
              description: "Type of question (e.g., multiple choice, boolean)",
              nullable: false,
            },
            difficulty: {
              type: Type.STRING,
              description: "Difficulty level of the question (easy, medium, hard)",
              nullable: false,
            },
            category: {
              type: Type.STRING,
              description: "Category of the question",
              nullable: false,
            },
            question: {
              type: Type.STRING,
              description: "The quiz question",
              nullable: false,
            },
            correct_answer: {
              type: Type.STRING,
              description: "The correct answer to the question",
              nullable: false,
            },
            incorrect_answers: {
              type: Type.ARRAY,
              description: "Array of incorrect answers",
              items: {
                type: Type.STRING,
              },
              nullable: false,
            },
          },
          required: [
            "type",
            "difficulty",
            "category",
            "question",
            "correct_answer",
            "incorrect_answers",
          ],
        },
        nullable: false,
      },
    },
    required: ["response_code", "results"],
  };

  const ai = new GoogleGenAI({ apiKey: '...' });  // Replace '...' with your actual API key

  try {
    const response = await ai.models.generateContent({
      model: "gemini-2.5-flash",
      contents: prompt,
      config: {
        responseMimeType: "application/json",
        responseSchema: schema,
      },
    });
    questions.value = JSON.parse(response.text);
    status.value = 'ready';
  } catch (error) {
    errorMessage.value = error;
    status.value = 'start';

  }
};

</script>

<template>

  <div id="app">

    <header>
      <div class="container">
        <img src="./assets/logo.png" alt="App Logo" class="logo">
        <h1>Generator Kuis</h1>
      </div>
    </header>
    <StartScreen v-if="status == 'start'" :errorMessage="errorMessage" @start-quiz="startQuiz" />
    <Loader v-if="status == 'loading'" />
    <Quiz v-if="status == 'ready'" @end-quiz="status = 'finished'" @store-answer="storeAnswer"
      :questions="questions.results" />
    <Result v-if="status == 'finished'" @restart-quiz="restartQuiz" :userAnswers="userAnswers" />

  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
