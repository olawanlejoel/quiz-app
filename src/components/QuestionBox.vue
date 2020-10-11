<template>
    <div class="box">
        <b-jumbotron>
            <template v-slot:lead>
            {{currentQuestion.question}}
            </template>

            <hr class="my-4">

            <b-list-group >
                <b-list-group-item
                    :key="index" 
                    v-for="(shuffledAnswer, index) in shuffledAnswers"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{shuffledAnswer}}
                </b-list-group-item>
            </b-list-group>

            <b-button 
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered "
            >
                Submit
            </b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash';

export default {
    name: "QuestionBox",
    props:{
        currentQuestion : Object,
        next: Function,
        increment : Function
    },
    data(){
        return{
            selectedIndex : null,
            shuffledAnswers:[],
            correctIndex:null,
            answered:false
            
        }
    },
    
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch:{
        currentQuestion:{
            immediate:true,
            handler(){
                this.selectedIndex = null
                this.answered=false
                this.shuffleAnswers()
            }
        }
        // (){
        //     this.selectedIndex = null
        //     this.shuffleAnswers()
        // }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index;
        },
        submitAnswer(){
            let isCorrect = false;

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true;
            }

            this.answered = true;

            this.increment(isCorrect);
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        answerClass(index){
            let className = null

                if(!this.answered && this.selectedIndex === index){
                    className = 'selected'
                }else if(this.answered && this.correctIndex === index){
                    className = 'correct'
                }else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                    className = 'incorrect'
                }

                    return className;
        }
    },
    // mounted(){
    //     this.shuffleAnswers()
    // }
}
</script>

<style scoped>
.box{
    margin-top:60px;
}
.list-group{
    margin-bottom:15px;
}
.list-group-item:hover{
    background: #eee;
    cursor: pointer;
}
.btn{
    margin:0 10px;
}
.selected{
    background-color: rgba(0, 0, 255, 0.788);
    color:#fff;
}
.correct{
    background-color: green;
    color:#fff;
}
.incorrect{
    background-color: red;
    color:#fff;
}
</style>