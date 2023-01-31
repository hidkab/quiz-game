<template>

  <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount"/>
    <h2 v-html="this.question"></h2>
    
    <template v-if="this.question">
      <template v-for="answer in this.answers" :key="answer">
        <input 
          :disabled="this.answerSubmited"
          type="radio" 
          name="options" 
          :value="answer"
          v-model="this.chosenAnswer"
        >
        <label v-html="answer"></label><br>
      </template>
      <button v-if="!this.answerSubmited" class="btn" @click="this.submitAnswer()" type="button">Send</button>
    </template>
    <div class="result" v-if="this.answerSubmited">
      <h4 v-if="this.chosenAnswer == this.correctAnswer" 
      v-html="'&#9989; Congratulations, the answer ' +  this.correctAnswer + ' is correct.'"> 
      </h4>
      <h4 v-else
      v-html="'I\'m sorry, you picked the wrong answer. The correct is ' + this.correctAnswer + '.'" > 
      </h4>
      <button class="btn next" type="button" @click="this.getNewQuestion()">Next question</button>
    </div>
</template>

<script>
import ScoreBoard from '@/components/ScoreBoard.vue'; 

export default {
  name: 'App',
  components: {
    ScoreBoard
  },

  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmited: false,
      winCount: 0,
      loseCount: 0
    }
  },

  computed: {
    answers() {
      var answers =JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },

  methods: {
   
    submitAnswer() {
      
      if (!this.chosenAnswer) {
        alert("Pick one of the answers");
      } else {
        this.answerSubmited = true;
        if (this.chosenAnswer === this.correctAnswer) {
          this.winCount++;
        } else {
          this.loseCount++;
        }
      }
    },

    getNewQuestion() {
      this.answerSubmited = false;
      this.chosenAnswer = undefined;
      this.question = undefined;
      this.axios
        .get('https://opentdb.com/api.php?amount=1&category=18')
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorrectAnswers = response.data.results[0].incorrect_answers;
          this.correctAnswer = response.data.results[0].correct_answer;
        });
    }
  },
  created() {
    this.getNewQuestion();
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 920px;
  margin:2.8rem auto;
}
input[type='radio'] {
  margin: 15px 5px ;
}
.btn {
  border: none;
  width: 80px;
  height: 30px;
  background-color: rgb(13, 99, 13);
  color: rgb(231, 225, 225);
  margin-top: 20px;
  border-radius: 3px;
}
.next {
  width: 120px;
}
</style>
