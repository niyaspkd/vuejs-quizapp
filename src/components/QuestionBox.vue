<template>
 <div>
  <b-jumbotron>
    <template slot="lead">
      {{CurrentQuestion.question}}
    </template>

    <hr class="my-4">
    <b-list-group>
    <b-list-group-item v-for="(answer,index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" :class="[!answered && selectedIndex === index ? 'selected' : 
    answered && correctIndex === index ? 'correct': 
    answered && correctIndex && selectedIndex === index ? 'incorrect': '']">

	{{ answer }}
    
    </b-list-group-item>

	</b-list-group>


      
    

    <b-button 
    variant="primary"
    @click="submitAnswer" :disabled="selectedIndex===null || answered" >Submit</b-button>
    <b-button @click="next" variant="success" href="#">Next</b-button>
  </b-jumbotron>
 </div>
</template>
<script >
	import _ from 'lodash'
	export default{
		props: {
			CurrentQuestion: Object,
			next: Function,
			increment: Function

		},
		data(){
			return{

				selectedIndex : null,
				shuffledAnswers : [],
				correctIndex: null,
				answered: false
				
			}

		},
		computed: {
			answers(){
				let answers = [...this.CurrentQuestion.incorrect_answers]
				answers.push(this.CurrentQuestion.correct_answer)
				return answers
			}

		},
		watch:{

			CurrentQuestion:{
				immediate: true,
				handler(){
				this.selectedIndex = null
				this.answered = false
				this.shuffleAnswers()

				}
			}

		},
		methods: {
			selectAnswer(index){
				this.selectedIndex = index
				

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
				let answers = [...this.CurrentQuestion.incorrect_answers, this.CurrentQuestion.correct_answer]
				this.shuffledAnswers = _.shuffle(answers)
				this.correctIndex = this.shuffledAnswers.indexOf(this.CurrentQuestion.correct_answer)
			}


		} ,
		

    }
	
</script>
<style scoped>
	.list-group{
		margin-bottom: 15px;
	}
	.list-group-item:hover{
		background: #EEE;
		cursor: pointer;
	}
	.btn{
		margin:0 5px;
	}
	.selected{
		background-color: lightblue;
	}
	.correct{
		background-color: green;
	}
	.incorrect{
		background-color: red;
	}

	
</style>