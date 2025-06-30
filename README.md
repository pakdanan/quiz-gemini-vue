# 🧠 Quiz Generator Vue.js Application Powered by Gemini

This project is a web-based quiz app built with [Vue.js 3](https://vuejs.org/) following the tutorial by **Daweb School**:📺 [Watch on YouTube](https://www.youtube.com/watch?v=jgjfMcKAqdA)

The app allows users to **generate quizzes using Gemini AI**, then **take and answer** those quizzes with real-time feedback.

## ✨ Features

- 🎯 Enter a topic and generate a quiz using Google Gemini API
- 📝 Automatically receive JSON-formatted quiz questions
- ✅ Answer questions interactively with immediate scoring
- 📈 Show results after completion
- 💡 Inspired by Daweb School's structured approach using Gemini + Vue

## 📚 Based on Daweb School’s Tutorial

This app follows the key concepts taught in the tutorial:
- Using Gemini's `model.generateContent()` with a **structured prompt**
- Validating Gemini's **JSON response** for quiz content
- Rendering questions dynamically in Vue
- Capturing user answers and calculating scores

## 🔧 Tech Stack

- Vue.js 3 + Composition API
- Google Gemini Pro API (via REST fetch)
- Vite (for fast development)

## 🚀 Getting Started

```bash
git clone https://github.com/USERNAME/vue-gemini-quiz.git
cd vue-gemini-quiz
npm install
