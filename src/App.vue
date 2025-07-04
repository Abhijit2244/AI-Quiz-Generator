<script setup>
import { GoogleGenAI } from "@google/genai";
import StartScreen from './components/StartScreen.vue';
import Quiz from "./components/Quiz.vue";
import Loader from "./components/Loader.vue";
import Result from "./components/Result.vue";
import { ref } from 'vue';
import { Type } from "@google/genai";


const difficulty = ref('medium'); // default
const questions = ref('');
const status = ref('start');
const userAnswers = ref([]);
const errorMessage = ref('');

const restartQuiz = () => {
  questions.value = '';
  status.value = 'start';
  userAnswers.value = [];
  errorMessage.value = '';
  difficulty.value = 'medium'; 
}

const storeAnswer = (answer) => {
  userAnswers.value.push(answer);
}

const responseSchema = {
  type: Type.OBJECT,
  properties: {
    response_code: {
      type: Type.NUMBER,
    },
    results: {
      type: Type.ARRAY,
      items: {
        type: Type.OBJECT,
        properties: {
          type: { type: Type.STRING },
          difficulty: { type: Type.STRING },
          category: { type: Type.STRING },
          question: { type: Type.STRING },
          correct_answer: { type: Type.STRING },
          incorrect_answers: {
            type: Type.ARRAY,
            items: { type: Type.STRING },
          },
        },
        propertyOrdering: [
          "type",
          "difficulty",
          "category",
          "question",
          "correct_answer",
          "incorrect_answers",
        ],
      },
    },
  },
  propertyOrdering: ["response_code", "results"],
};


const startQuiz = async (topic) => {
  status.value = 'loading';
  const ai = new GoogleGenAI({ apiKey: import.meta.env.VITE_GEMINI_API_KEY, });
  questions.value = "Loading Questions..."
  async function main() {
    const response = await ai.models.generateContent({
      model: "gemini-2.5-flash",
      contents: `Create 5 ${difficulty.value} multiple-choice quiz questions about "${topic}"`,
      config: {
        responseMimeType: "application/json",
        responseSchema: responseSchema,
        thinkingConfig: {
          thinkingBudget: 0, // Disables thinking
        },
      }
    });
    questions.value = JSON.parse(response.text);
    status.value = 'ready'
  }
  try {
    await main();
  } catch (error) {
    errorMessage.value = error;
    status.value = 'start';

  }


}

</script>

<template>
  <div id="app">
    <header>
      <div class="container">
        <img src="./assets/logo.png" alt="" class="logo">
        <h1>Quiz Generator</h1>
      </div>
    </header>

    <StartScreen v-if="status == 'start'" v-model:difficulty="difficulty" @start-quiz="startQuiz" />
    <Loader v-if="status == 'loading'" />
    <Quiz v-if="status == 'ready'" @end-quiz="status = 'finished'" @store-answer="storeAnswer"
      :questions="questions.results" />
    <Result v-if="status == 'finished'" @restart-quiz="restartQuiz" :userAnswers="userAnswers" />

  </div>
</template>
