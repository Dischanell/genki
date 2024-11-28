<script setup lang="ts">
import { ref, computed } from 'vue'
import { katakana } from '@/katakana';

interface KatakanaCharacter {
	kana: string
	romaji: string
}

const answerInput = ref('')
const correctAnswers = ref<KatakanaCharacter[]>([])
const incorrectAnswers = ref<KatakanaCharacter[]>([])

const currentCharacterIndex = ref(0)
const currentCharacter = computed(() => shuffledKatakana[currentCharacterIndex.value])

const shuffleArray = (array: KatakanaCharacter[]) => {
	for (let i = array.length -1; i > 0; i--) {
		const j = Math.floor(Math.random() * (i + 1))
		;[array[i], array[j]] = [array[j], array[i]]
	}

	return array
}

const shuffledKatakana = shuffleArray(katakana)

const checkAnswer = () => {
	if (!answerInput.value.trim()) return // Prevent checking the answer if input is empty

	const isCorrect = answerInput.value.toLowerCase() === currentCharacter.value.romaji
	;(isCorrect ? correctAnswers : incorrectAnswers).value.push(currentCharacter.value)

	currentCharacterIndex.value++
	answerInput.value = ''
}
</script>

<template>
	<main>
		<div class="container">
			<div v-if="currentCharacterIndex < shuffledKatakana.length" class="input-container">
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
		</div>
	</main>
</template>

<style scoped>
.container{
	margin: 2rem;
	padding: 1rem;
	display: flex;
	flex-direction: column;
	align-items: center;
}

.input-container {
	display: flex;
	flex-direction: column;
	align-items: center;
	gap: 1rem;
}

input {
	background-color: #ffffffcc;
	color: black;
}

input:focus {
	background-color: transparent;
	color: white;
}
</style>
