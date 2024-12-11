<template>
  <div class="container">
    <img src="./assets/logo.png"/>
    <hr>
    <h1>Microphones can b used not only to pick up sound, but also to project to a speaker.</h1>

    <label><input type="radio" name="option" value="true"/>True</label>
    <label> <input type="radio" name="option" value="false"/>False</label>
    <button>Enviar</button>
  </div>
</template>

<script setup>
import axios from 'axios'
import { onMounted, ref} from 'vue'

let quiz = ref({
  question: null,
  incorrect_answers: '',
  correct_answer: ''
})

async function loadQuiz () {
  const response = await axios.get('https://opentdb.com/api.php?amount=1&category=18')
  const data = response.data.results[0]
  this.quiz.value.question = data.question
}
console.log(quiz)

onMounted(() => loadQuiz)
</script>
