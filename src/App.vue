<template>
  <div id="app">
    <Header :numcorrect="numcorrect" :totalnum="totalnum" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <questionbox
            v-if="questions.length"
            :currentquestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/header.vue";
import questionbox from "./components/questionbox.vue";

export default {
  name: "App",
  components: {
    Header,
    questionbox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numcorrect: 0,
      totalnum: 0,
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(iscorrect) {
      console.log(iscorrect)
      if (iscorrect) {
        this.numcorrect++;
        console.log(this.numcorrect);
      }
      this.totalnum++;
      console.log(this.totalnum);
    }
  },

  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
