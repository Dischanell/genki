<script setup lang="ts">
import { ref } from 'vue';

const katakana = [
  { kana: 'ア', romaji: 'a' },
  { kana: 'イ', romaji: 'i' },
  { kana: 'ウ', romaji: 'u' },
  { kana: 'エ', romaji: 'e' },
  { kana: 'オ', romaji: 'o' }
];

const userAnswers = ref(new Array(katakana.length).fill(''));

const checkAnswer = (index: number) => {
  const isCorrect = userAnswers.value[index] === katakana[index].romaji;
  console.log(isCorrect ? 'Correct!' : `Incorrect. The correct answer is: ${katakana[index].romaji}`);
  // Add visual feedback here based on isCorrect
}
</script>

<template>
  <main>
    <div class="quiz-item" v-for="(char, index) in katakana" :key="index">
      <p>{{ char.kana }}</p>
      <input type="text" v-model="userAnswers[index]" @keyup.enter="checkAnswer(index)">
    </div>
  </main>
</template>