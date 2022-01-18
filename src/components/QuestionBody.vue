<template>
  <div>
    <b-card-header :class="variant" class="d-flex justify-content-between border-bottom-0">
    </b-card-header>
    <b-card-body>
      <b-card-text class="font-weight-bold" v-html="questionData.question"></b-card-text>
    </b-card-body>
  </div>
</template>

<script>
export default {
  name: 'QuestionBody',
  props: {
    /** Object containing question data as given by API. */
    questionData: {
      required: true,
      type: Object
    }
  },
  data() {
    return {
      variant: 'custom-info'
    }
  },
  methods: {
    /** Invoked on mounted().
     * Sets background color of card header based on question difficulty.
     * @public
     */
  },
  mounted() {
    
  }
}
</script>

<docs>
Simple component displaying question category, difficulty and question text. 
Used on both Question component and Answer component.
</docs>