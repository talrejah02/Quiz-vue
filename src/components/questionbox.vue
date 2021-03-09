<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currentquestion.question }}
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectanswers(index)"
          :class="answerclass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        @click="submit"
        :disabled="selectedindex === null || answered"
      >
        SUBMIT
      </b-button>
      <b-button @click="next" variant="success" href="#">NEXT</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentquestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedindex: null,
      shuffledAnswers: [],
      correctindex: null,
      answered: false ,
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentquestion.incorrect_answers];
      answers.push(this.currentquestion.correct_answer);
      return answers;
    },
  },
  methods: {
    selectanswers(index) {
      this.selectedindex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentquestion.incorrect_answers,
        this.currentquestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctindex = this.shuffledAnswers.indexOf(
        this.currentquestion.correct_answer
      );
    },
    submit() {
      let iscorrect = false;
      
      if (this.selectedindex === this.correctindex) {
        iscorrect = true;
        console.log(iscorrect);
      }
       this.answered=true;
      this.increment(iscorrect);
      
    },
   answerclass(index){
       let answerclass=''
      if(!this.answered && this.selectedindex === index){
        answerclass='selected'
      }else if(this.answered && this.correctindex === index) {
         answerclass='correct'
      }else if(this.answered && this.selectedindex === index && this.correctindex !== index) {
        answerclass='incorrect'
      }
       return answerclass

      
    }
  },
  watch: {
    currentquestion: {
      immediate: true,
      handler() {
        this.selectedindex = null;
        this.answered=false;
        this.shuffleAnswers();
      },
    },
    //
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.btn {
  margin: 0 8px;
}
.list-group-item:hover {
  background-color: rgb(211, 190, 190);
  cursor: pointer;
}
.selected {
  background-color: darkorchid;
}
.correct {
  background-color: greenyellow;
}
.incorrect {
  background-color: red;
}
</style>