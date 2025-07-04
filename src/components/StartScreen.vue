<script setup>
import { ref, defineEmits, defineProps } from 'vue';

const props = defineProps({
  errorMessage: String,
  difficulty: String,
});
const emit = defineEmits(['start-quiz', 'update:difficulty']);
const topic = ref('');

const start = () => {
  if (topic.value.trim()) {
    emit('start-quiz', topic.value.trim());
  }
};

const setDifficulty = (level) => {
  emit('update:difficulty', level);
};
</script>

<template>
  <section class="start-screen">
    <div class="card">
      <h2>🎓 Start Your AI Quiz</h2>

      <input
        v-model="topic"
        type="text"
        placeholder="Enter your quiz topic (e.g., JavaScript, History)"
      />

      <h3>Select Difficulty</h3>
      <div class="difficulty-toggle">
        <button
          v-for="level in ['easy', 'medium', 'hard']"
          :key="level"
          :class="{ active: difficulty === level }"
          @click="setDifficulty(level)"
        >
          {{ level }}
        </button>
      </div>

      <button class="start-btn" @click="start" :disabled="!topic">
        🚀 Start Quiz
      </button>

      <p class="error" v-if="errorMessage">
        ⚠️ {{ errorMessage }}
      </p>
    </div>
  </section>
</template>

<style scoped>
.start-screen {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: calc(100vh - 80px);
  background: linear-gradient(to right, #dfe9f3, #ffffff);
  padding: 20px;
}

.card {
  background-color: white;
  padding: 2rem;
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 450px;
  text-align: center;
}

h2 {
  margin-bottom: 1.5rem;
  font-size: 1.8rem;
  color: #333;
}

input[type="text"] {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ccc;
  border-radius: 12px;
  font-size: 1rem;
  margin-bottom: 1.5rem;
  outline: none;
  transition: 0.3s;
}

input[type="text"]:focus {
  border-color: #2196f3;
  box-shadow: 0 0 0 2px rgba(33, 150, 243, 0.2);
}

.difficulty-toggle {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-bottom: 1.5rem;
}

.difficulty-toggle button {
  padding: 0.5rem 1rem;
  border-radius: 20px;
  border: 1px solid #ccc;
  background-color: #f0f0f0;
  font-weight: 500;
  text-transform: capitalize;
  transition: all 0.3s ease;
  cursor: pointer;
}


.difficulty-toggle button.active {
  background-color: #2196f3;
  color: black;
  border-color: #2196f3;
}

.start-btn {
  background-color: #4caf50;
  color: white;
  font-weight: bold;
  font-size: 1rem;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.start-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.start-btn:not(:disabled):hover {
  background-color: #43a047;
}

.error {
  margin-top: 1rem;
  color: #d32f2f;
  font-size: 0.95rem;
}
</style>
