</template>

<script>
import QuestionBody from './QuestionBody'

export default {
  name: 'Answer',
  props: {
    /** Question object containing questionData, possible answers, and user answer information. */
    question: {
      required: true,
      type: Object
    }
  },
  components: {
    QuestionBody
  }
}
</script>

<style scoped>
.answer-card >>> .card-header {
  border-radius: 0;
}
</style>