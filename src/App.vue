<template>
  <div class="container">
    <img src="./assets/logo.png"/>

    <score-body :winCount="winCount" :loseCount="loseCount"/>

    <template v-if="quiz.question">
      <h1 v-html="quiz.question"></h1>
      <form @submit.prevent="submitAnswer()" class="form-answers">
        <div class="chooses">
          <template v-for="(answer,index) in answers" :key="index">
            <div class="option">
              <input type="radio"
                v-model="chooseAnswer"
                :value="answer"
                :disabled="answerSubmitted"/>
              <label v-html="answer"></label>
            </div>
          </template>
        </div>
        <button type="submit" v-if="!answerSubmitted">Enviar</button>

      </form>
    </template>
    <section class="result" v-if="answerSubmitted">
      <h4 v-if="quiz.correctAnswer === chooseAnswer">&#9989; Crongratulation, the answer "{{ quiz.correctAnswer }}" is correct</h4>
      <h4 v-else>&#10060; I'm Sorry, you picked the wrong answer. The correct is "{{ quiz.correctAnswer }}"</h4>
      <button @click="nextQuiz()">Next Question</button>
    </section>
  </div>
</template>

<script>
import ScoreBody from './components/ScoreBody.vue'

export default {
  components: {ScoreBody},
  data() {
    return {
      quiz: {
        question: undefined,
        incorrectAnswers: undefined,
        correctAnswer: undefined
      },
      chooseAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0
    }
  },
  methods: {
    submitAnswer () {
      if(!this.chooseAnswer) {
        alert('Pick one of the option!')
      } else {
        this.answerSubmitted = true
        if(this.chooseAnswer === this.quiz.correctAnswer) {
          this.winCount ++
        } else {
          this.loseCount ++
        }
      }
    },
    nextQuiz() {
      this.getNewQuiz()
    },
    async getNewQuiz() {

      this.answerSubmitted = false
      this.chooseAnswer = undefined
      this.quiz = {}

      const response = await this.axios.get('https://opentdb.com/api.php?amount=1&category=18')
      const data = response.data.results[0]
      this.quiz.question = data.question
      this.quiz.correctAnswer = data.correct_answer
      this.quiz.incorrectAnswers = data.incorrect_answers
    }
  },
  computed: {
    answers() {
      let answers = [...this.quiz.incorrectAnswers]
      answers.push(this.quiz.correctAnswer)
      return answers.toSorted()
    }
  },
  async created () {
    this.getNewQuiz()
  }
}

</script>


<!-- let quiz = ref({
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

onMounted(() => loadQuiz) -->
