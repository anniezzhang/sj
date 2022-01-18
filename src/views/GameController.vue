<template>
  <div class="h-100">
    <LoadingIcon v-if="loading"></LoadingIcon>
    <Question :question="currentQuestion" @answer-submitted="onAnswerSubmit" v-else></Question>
  </div>
</template>

<script>
import EventBus from '../eventBus'
import Question from '../components/Question.vue'
import LoadingIcon from '../components/LoadingIcon.vue'
import axios from 'axios'
//import jsonData from '../static/questions.json'
export default {
  name: 'GameController',
  props: {
    /** Number of questions */
    number: {
      default: '10',
      type: String,
      required: true
    },
    /** Id of category. Empty string if not included in query */
    category: String,
    /** Difficulty of questions. Empty string if not included in query */
    difficulty: String,
    /** Type of questions. Empty string if not included in query */
    type: String
  },
  components: {
    Question,
    LoadingIcon
  },
  data() {
    return {
      // Array of custom question objects. See setQuestions() for format
      questions: [],
      currentQuestion: {},
      // Used for displaying ajax loading animation OR form
      loading: true
	
    }
  },
  created() {
    this.fetchQuestions()
  },
  methods: {
    /** Invoked on created()
     * Builds API URL from query string (props).
     * Fetches questions from API.
     * "Validates" return from API and either routes to MainMenu view, or invokes setQuestions(resp).
     * @public
     */

    fetchQuestions() {

	//	let url = '../static/questions.json'
      // let url = `https://opentdb.com/api.php?amount=${this.number}`
      // if (this.category)   url += `&category=${this.category}`
      // if (this.difficulty) url += `&difficulty=${this.difficulty}`
      // if (this.type)       url += `&type=${this.type}`

      axios.get("/js/questions.json")
	.then(resp=>resp.data)
        .then(resp => {
		console.log("resp is as below" + resp.results)
          if (resp.response_code === 0) {
            this.setQuestions(resp)
          } else {
            EventBus.$emit('alert-error', 'Bad game settings. Try another combination.')
            this.$router.replace({ name: 'home' })
          }
        })
    },
    /** Takes return data from API call and transforms to required object setup. 
     * Stores return in $root.$data.state.
     * @public
     */
    setQuestions(resp) {
      resp.results.forEach(qst => {
        const answers = [qst.correct_answer, ...qst.incorrect_answers]
        const question = {
          questionData: qst,
          answers: answers,
          userAnswer: null,
          correct: null
        }
        this.questions.push(question)
      })
      this.$root.$data.state = this.questions
      this.currentQuestion = this.questions[0]
      this.loading = false
    },
    /** Called on submit.
     * Checks if answer is correct and sets the user answer.
     * Invokes nextQuestion().
     * @public
     */
    onAnswerSubmit(answer) {
      if (this.currentQuestion.questionData.correct_answer === answer) {
        this.currentQuestion.correct = true
      } else {
        this.currentQuestion.correct = false
      }
      this.currentQuestion.userAnswer = answer
      this.nextQuestion()
    },
    /** Filters all unanswered questions, 
     * checks if any questions are left unanswered, 
     * updates currentQuestion if so, 
     * or routes to "result" if not.
     * @public
     */
    nextQuestion() {
      const unansweredQuestions = this.questions.filter(q => !q.userAnswer)
      if (unansweredQuestions.length > 0) {
        this.currentQuestion = unansweredQuestions[0]
      } else {
        this.$router.replace({ name: 'result' })
      }
    }
  }
}
</script>