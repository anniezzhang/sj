<template>
  <div>
    <QuestionBody :questionData="question.questionData"></QuestionBody>

    <b-card-body class="pt-0">
      <hr>
      <b-form @submit="onSubmit">
        <b-form-group
          class="text-left"
        >
          <b-form-radio 
            v-for="(ans, index) of question.answers" 
            :key="index" 
            v-model="answer" 
            :value="ans"
          >
            <div v-html="ans"></div>
          </b-form-radio>
        </b-form-group>

        <b-button type="submit" class="custom-success">Submit</b-button>
		<b-alert variant="danger" :show="showAlert">Please choose an answer.</b-alert>
      </b-form>
    </b-card-body>
  </div>
</template>

<script>
import QuestionBody from './QuestionBody'

export default {
  name: 'Question',
  props: {
    /** Question object containing questionData, possible answers, and user answer information. */
    question: {
      required: true,
      type: Object
    }
  },
  components: {
    QuestionBody
  },
  data() {
    return {
    answer: null,
	showAlert: false
    }
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault()
      if (this.answer) {
        /** Triggered on form submit. Passes user answer.
        * @event answer-submitted
        * @type {number|string}
        * @property {string}
        */
		this.showAlert = false
        this.$emit('answer-submitted', this.answer)
        this.answer = null
      }else{
		this.showAlert = true
	}
    } 
  }
}
</script>
