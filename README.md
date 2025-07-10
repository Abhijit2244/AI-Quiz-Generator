# 🤖 AI Quiz Generator

An intelligent quiz generator powered by **Vue.js** and **Google's Gemini API** (Gemini 2.5 Flash). Users can instantly generate topic-based multiple-choice quizzes — perfect for learners, educators, or quiz enthusiasts.

---

## 🚀 Features

- 🔮 AI-generated quiz questions on any topic
- 📚 Multiple-choice format with medium difficulty
- ⚡ Fast and responsive Vue 3 + Vite frontend
- 💡 Real-time feedback and results
- 🔐 API key securely handled via `.env` file
- 🧼 Clean component structure (`StartScreen`, `Quiz`, `Result`, `Loader`)

---
<!-- 
## 🖼️ Preview

![AI Quiz Generator Screenshot](./public/preview.png) <!-- Replace this with an actual image path -->
<!-- 
---
 -->
## 🛠️ Tech Stack

- **Vue 3 (Composition API)**
- **Vite**
- **Google Generative AI SDK (`@google/genai`)**
- **JavaScript / HTML / CSS**

---

## 📦 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Abhijit2244/AI-Quiz-Generator.git
cd ai-quiz-generator
```

### 2. Install Dependencies

```bash
npm install
```
### 3. Setup Environment Variable

Create a .env file in the root folder
```bash
VITE_GEMINI_API_KEY=your_gemini_api_key_here
```
### ▶️ Run the App

```bash
npm run dev
```