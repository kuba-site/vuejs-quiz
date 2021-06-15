<template>
    <div class="question-box-container">
      <b-jumbotron>
        <template #lead>
          {{ currentQuestion.question }}
        </template>

        <hr class="my-4">

        <b-list-group>
          <b-list-group-item
              v-for="(answer, index) in answers"
              :key="index"
              @click="selectAnswer(index)"
              :class="answerClass(index)"
      >
        {{answer}}
      </b-list-group-item>
    </b-list-group>

    <b-button
        variant="primary" href="#"
        @click="submitAnswer()"
        :disabled="selectedIndex === null || answered === true"
    >
      Submit
    </b-button>
    <b-button @click="next" variant="success" href="#">Next question</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
name: "QuestionBox",
props: {
currentQuestion: Object,
next: Function,
increment: Function
},
data() {
return {
  selectedIndex: null,
  shuffledAnswers: [],
  correctIndex: null,
  answered: false
}
},
computed: {
answers() {
  let answers = [...this.currentQuestion.incorrect_answers]
  answers.push(this.currentQuestion.correct_answer)
  return answers
}
},
watch: {
currentQuestion: {
  immediate: true,
  handler() {
    this.selectedIndex = null
    this.answered = false
    this.shuffleAnswers()
  }
}
},
methods: {
  selectAnswer(index) {
    this.selectedIndex = index
  },
  shuffleAnswers() {
    let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
    this.shuffledAnswers = _.shuffle(answers)
    this.correctIndex = answers.indexOf(this.currentQuestion.correct_answer)
  },
  submitAnswer() {
    let isCorrect = false;

    if (this.selectedIndex === this.correctIndex) {
      isCorrect = true;
    }
    this.answered = true;
    this.increment(isCorrect);
  },
  answerClass(index) {
    let answerC = ''
    if (index === this.selectedIndex && this.answered === false){
        answerC = 'selected'
      }

    else if (this.answered && index === this.correctIndex){
      answerC = 'correct'
    }

    else if (this.answered && index === this.selectedIndex && index != this.correctIndex) {
      answerC = 'incorrect'
    }
    else answerC = ''
    return answerC
  }
},
}
</script>

<style scoped>

.list-group {
margin-bottom: 15px;
}
.selected {
background-color: lightblue;
}
.correct {
background-color: lightgreen;
}
.incorrect {
background-color: lightcoral;
}
.list-group-item:hover {
background-color: #EEE;
cursor: pointer;
}
.btn {
margin: 0 5px;
}

</style>

