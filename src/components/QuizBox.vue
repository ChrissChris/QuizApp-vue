<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group
        v-for="(answer, index) in answers"
        :key="index"
        @click="selectAnswer(index)"
        @click.prevent="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : '']"
      >
        <b-list-group-item>{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary" 
      @click="submitAnswer"
      :disabled="selectedIndex === null || answered"
      >Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">Next Question</b-button>
    </b-jumbotron>
  </div>
</template>



<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },

  data: function() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },

  
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },

  watch: {
   currentQuestion: {
    immediate: true,
    handler() {
      this.selectedIndex = null
      this.answered = false
      this.shuffledAnswers()
    }

   }
  },

  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
     
    submitAnswer(){
       let isCorrect = false

       if(this.selectedIndex === this.correctIndex){
         isCorrect = true
       }
      
      this.answered = true
      this.increment(isCorrect)
    },

    shuffleAnswers(){
     let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
     this.shuffleAnswers= _.shuffle(answers)
     this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    }
  },
  
};
</script>



<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: greenyellow;
}

.incorrect {
  background-color: red;
}

.btn {
  margin: 0 5px;
}
</style>