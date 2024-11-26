<script setup lang="ts">
import { ref, computed } from 'vue'
import { katakana } from '@/katakana';

const shuffledKatakana = katakana.sort((a, b) => .5 - Math.random())
let currentCharacterIndex = ref(0)

const answerInput = ref('')
const correctAnswers = ref<{kana: string, romaji: string}[]>([])
const incorrectAnswers = ref<{kana: string, romaji: string}[]>([])

const currentCharacter = computed(() => shuffledKatakana[currentCharacterIndex.value])

const checkAnswer = () => {
	if (!answerInput.value.trim()) return // Prevent checking the answer if input is empty

	if (answerInput.value.toLowerCase() === currentCharacter.value.romaji) {
		correctAnswers.value.push(currentCharacter.value)
	} else {
		incorrectAnswers.value.push(currentCharacter.value)
	}
	currentCharacterIndex.value++
	answerInput.value = ''
}
</script>

<template>
	<main>
		<div v-if="currentCharacterIndex < shuffledKatakana.length">
			<h3>{{ currentCharacter.kana }}</h3>
			<input type="text" v-model="answerInput" @keydown.enter="checkAnswer()" autofocus>
		</div>
		<div v-else>
			<h2>Quiz Complete!</h2>
			<h2 v-if="incorrectAnswers.length === 0">おめでとうございます! You have mastered Katakana.</h2>
			<p>{{ `${correctAnswers.length}/${shuffledKatakana.length} correct answers.` }}</p>
			<div v-if="incorrectAnswers.length > 0">
				<p>Incorrect Answers:</p>
				<ul>
					<li v-for="char in incorrectAnswers" :key="char.kana">{{ char.kana }}: {{ char.romaji }}</li>
				</ul>
			</div>
		</div>
	</main>
</template>