<template lang="html">
  <div class="question-container">

  <b-jumbotron>

    <template slot="lead">
      {{currentQuestions.question}}
    </template>

    <hr class="my-4" />

    <b-list-group>
        <b-list-group-item
        v-for="(answer,index) in shuffledAnswers"
        :key="index"
        @click.prevent="selectAnswer(index)"
        :class="answerclass(index)"
        >
        <!--:class="[
        !answered && selectedIndex === index ? 'selected' :
        answered && correctIndex === index ? 'correct' :
        answered && selectedIndex === index && correctIndex !== index ? 'incorrect' : ' ']"-->
        {{ answer }}
        </b-list-group-item>
    </b-list-group>

  <!--<b-button @click="prev('previous','answerclass')" variant="success" >Previous</b-button>-->
    <b-button variant="primary"  @click="submitAnswer" :disabled="selectedIndex==null ||answered ">Submit</b-button>
    <b-button @click="next" variant="success" :disabled="selectedIndex==null ||answered==false" >Next</b-button>
  </b-jumbotron>

  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestions: Object,
    next: Function,
    previous: Function,
    increment:Function
  },
  data() {
    return{
      selectedIndex:null,
      correctIndex:null,
      shuffledAnswers:[],
      answered:null
    }
  },
  computed: {
    answers(){
      let answers=[...this.currentQuestions.incorrect_answers]
      answers.push(this.currentQuestions.correct_answer)
      return answers
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex=index
    },
    shuffleAnswers() {
      let answers=[...this.currentQuestions.incorrect_answers,this.currentQuestions.correct_answer]
      this.shuffledAnswers= _.shuffle(answers)
      this.correctIndex=this.shuffledAnswers.indexOf(this.currentQuestions.correct_answer)
    },
    submitAnswer() {
      let isCorrect=false

      if(this.selectedIndex===this.correctIndex){
        isCorrect=true
      }
      this.answered=true
      this.increment(isCorrect)
    },
    answerclass(index) {
      let answerclass=" "
       if(!this.answered && this.selectedIndex === index) {
         answerclass='selected'
       }

       else if (this.answered && this.correctIndex === index) {
         answerclass='correct'
       }
      else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerclass='incorrect'
      }

      return answerclass
    }
   /*  prev() {
      this.previous()
      this.answerclass(thisindex)
    }*/
  },
  watch: {
    /*currentQuestions() {
      this.selectedIndex=null
      this.shuffleAnswers()
    }*/
    currentQuestions: {
      immediate:true,
      handler() {
        this.selectedIndex=null,
        this.shuffleAnswers(),
        this.answered=false   //to enable the submit button
      }
    }
  }

}
  /*mounted() {
    this.shuffleAnswers()           used to start shuffleAnswers function at the begining
                                    when in watch currentQuestions is used as function
  }*/

</script>

<style lang="css" scoped>
.list-group{
  margin-bottom:15px;
}
.list-group-item:hover{
  background: #EEE;
  cursor: pointer;
}
.btn{
  margin:0px 15px;
}
.selected{
  background-color:lightblue;
}
.correct{
  background-color: lightgreen;
}
.incorrect{
  background-color: red;
}

</style>
