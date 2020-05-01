<template>
  <div class="question-box-class">
    <b-jumbotron>
      <template v-slot:lead>
        {{ question.question }}
        Questions
      </template>
      <hr class="my-4">
      <b-list-group>
        <b-list-group-item 
          v-for="(ans, index) in answers" 
          :key="index"
          @click="selectedAnswer(index)"
          :class="answerClass(index)"
          :style="styleObj"
          >
          
          {{ ans }}
        </b-list-group-item>
      </b-list-group>
      <b-button  variant="primary" href="#"
        @click="submit"
        :disabled="selected_index === null || isAnswered"  
      >
        Submit
      </b-button>
      <!-- <b-button @click="next" variant="success" href="#">
        Next
      </b-button> -->
    </b-jumbotron>
  </div>
</template>

<script>
import shuffle from 'lodash/shuffle'
export default {
  props: {
    question: Object,
    next: Function,
    increment: Function
  },
  data(){
    return {
      selected_index: null,
      shuffled_answers: [],
      isAnswered: false,
      correctIndex: null
    }
  },
  watch:{
    question:{
      immediate: true,
      handler(){
        this.selected_index = null
        this.shuffleAnswers()
        this.isAnswered = false
      }
    }
  },
  // mounted(){
  //   this.shuffleAnswers()
  // },
  methods:{
    selectedAnswer(index){
      if(!this.isAnswered){
        this.selected_index = index
      }
    },
    shuffleAnswers(){
      let ans_arr = [...this.question.incorrect_answers, this.question.correct_answer]
      this.shuffled_answers = shuffle(ans_arr)
      this.correctIndex = this.shuffled_answers.indexOf(this.question.correct_answer)
    }
    ,
    submit(){
      let isCorrect = false
      if (this.selected_index === this.correctIndex){
        isCorrect = true
      }
      this.increment(isCorrect)
      this.isAnswered = true
      // sleep(1000)
      setTimeout(function() {
        this.next()}.bind(this), 2000
        )
        // console.log(isCorrect);

    },
    answerClass(index){
      let answerClass = ''
      if (!this.isAnswered && this.selected_index === index){
        answerClass = 'selected'
      } else if(this.isAnswered && this.correctIndex === index){
        answerClass = 'correct'
      } else if(this.isAnswered && this.selected_index === index && this.correctIndex != index){
        answerClass = 'incorrect'
      }
      return answerClass
    }
    
  },
  computed:{
    answers(){
      const ans_array = [...this.question.incorrect_answers]
      ans_array.push(this.question.correct_answer)
      return ans_array
    },
    styleObj(){
      if (!this.isAnswered){
        return {
          '--button-color-hover': '#EEE'
        }
      }else{
        return {
          '--button-color-hover': 'white'
        }
      }
    }
  }
}
</script>

<style scoped>
.list-group{
  margin-bottom: 15px;
}

.list-group-item:hover{
  background-color: var(--button-color-hover);
  cursor: pointer;
}

.selected{
  background-color: lightblue;
}

.correct{
  background-color: lightgreen;
}

.incorrect{
  background-color: red;
}

.btn{
  margin: 0px 5px;
}
</style>