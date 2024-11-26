<script setup lang="ts">
import { ref, computed } from 'vue'

const katakana = ref([
  { kana: 'ア', romaji: 'a' },
  { kana: 'イ', romaji: 'i' },
  { kana: 'ウ', romaji: 'u' },
  { kana: 'エ', romaji: 'e' },
  { kana: 'オ', romaji: 'o' }
])

const currentCharacter = computed(() => {
  return katakana.value[Math.floor(Math.random() * katakana.value.length)]
})

const answer = ref('')
const errorMessage = ref('')
const isGameOver = ref(false)

const checkAnswer = () => {
	if (answer.value === currentCharacter.value.romaji) {
		katakana.value = katakana.value.filter(char => char !== currentCharacter.value)
		if (katakana.value.length === 0) {
		  isGameOver.value = true
		}
		answer.value = ''
		errorMessage.value = ''
	} else {
		errorMessage.value = currentCharacter.value.romaji
		answer.value = ''
	}
}
</script>

<template>
  <main>
    <div v-if="!isGameOver">
      <p>{{ currentCharacter.kana }}</p>
      <p class="error-message">{{ errorMessage }}</p>
      <input type="text" v-model="answer" @keyup.enter="checkAnswer()">
    </div>
    <div v-else>
      <h2>おめでとうございます! You've mastered katakana!</h2>
    </div>
  </main>
</template>
