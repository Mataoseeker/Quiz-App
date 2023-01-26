<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
    id: 1,
    question: 'What is my middle name?',
    answer: 1,
    options: [
      'Martha',
      'Ochuko',
      'Omonigho'
    ],
    selected: null
  },
  {
    id: 2,
    question: 'When is my birth Month?',
    answer: 0,
    options: [
      'July',
      'June',
      'January'
    ],
    selected: null
  },
  {
    id: 3,
    question: 'What is my favourite thing to do?',
    answer: 2,
    options: [
      'Coding',
      'Reading',
      'Watching Movies'
    ],
    selected: null
  },
  {
    id: 4,
    question: 'What is my favourite food?',
    answer: 1,
    options: [
      'Yam',
      'Jollof Rice',
      'Beans & Plantain'
    ],
    selected: null
  },
  {
    id: 5,
    question: 'What is my favourite drink?',
    answer: 2,
    options: [
      'Coke',
      'Fanta',
      'No Favourite'
    ],
    selected: null
  },
  {
    id: 6,
    question: 'What is my favourite colour?',
    answer: 2,
    options: [
      'Red',
      'Blue',
      'Black'
    ],
    selected: null
  },
  {
    id: 7,
    question: 'What is my favourite animal?',
    answer: 1,
    options: [
      'Dog',
      'Cat',
      'Rabbit'
    ],
    selected: null
  },
  {
    id: 8,
    question: 'What is my favourite sport?',
    answer: 0,
    options: [
      'Badminton',
      'Basketball',
      'Volleyball'
    ],
    selected: null
  },
  {
    id: 9,
    question: 'What is my favourite music genre?',
    answer: 2,
    options: [
      'Rap',
      'Afrobeat',
      'R&B'
    ],
    selected: null
  },
  {
    id: 10,
    question: 'What is my favourite genre of movies?',
    answer: 1,
    options: [
      'Comedy',
      'Horror',
      'Sci-fi'
    ],
    selected: null
  },
  {
    id: 11,
    question: 'What is my favourite TV show?',
    answer: 2,
    options: [
      'Friends',
      'The Big Bang Theory',
      'Two and a half Men'
    ],
    selected: null
  },
  {
    id: 12,
    question: 'What kind of person am i?',
    answer: 0,
    options: [
      'An introvert',
      'An Extrovert',
      'Mixture of both'
    ],
    selected: null
  },
  {
    id: 13,
    question: 'What University did i attend?',
    answer: 2,
    options: [
      'University of Lagos',
      'University of Ibadan',
      'University of Benin'
    ],
    selected: null
  },
  
])

const quizCompleted = ref(false)

const currentQuestion = ref(0)

const score = computed (() => {
  let value = 0
  questions.value.map(question => {
    if(question.answer == question.selected){
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const setAnswer = evt => {
 questions.value[currentQuestion.value].selected = evt.target.value
 evt.target.value = null
}

const nextQuestion = () => {
  if(currentQuestion.value < questions.value.length - 1){
    currentQuestion.value++
  }else{
    quizCompleted.value = true
  }
}


</script>

<template>
 
 <br />
 <main class="app">
    <h1>How well do you know Martha?</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
      <div class="question">
        <h2>{{ getCurrentQuestion.question }}</h2>
        <span class="score">
          score {{ score }} / {{ questions.length }}
        </span>
      </div>
    </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" :key="index"
         :class="`option ${
          getCurrentQuestion.selected == index 
          ? index == getCurrentQuestion.answer 
            ? 'correct' 
            : 'incorrect'
          : ''
        
        } ${
          getCurrentQuestion.selected != null &&
          index != getCurrentQuestion.selected
          ? 'disabled'
          : ''
        }`">
          <input type="radio" :name="getCurrentQuestion.index" :value="index"
          v-model="getCurrentQuestion.selected"
          :disabled="getCurrentQuestion.selected" 
          @change="setAnswer">
          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="nextQuestion"
      :disabled="!getCurrentQuestion.selected">
        {{ 
        getCurrentQuestion.index == questions.length - 1
        ? 'Submit Quiz'
        : getCurrentQuestion.selected == null
          ? 'Select an option'
          : 'Next Question'
        }}
      </button>
    </section>

    <section v-else>
      <h2>Quiz Completed</h2>
      <p>Your score is {{ score }} / {{ questions.length }}</p>
    </section>
 </main>
</template>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body{
  background-color: #271C36;
  color: #fff;
}
.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;

}
h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz{
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
}
.quiz-info{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.quiz-info .question {
  color: #8F8F8F;
  font-size: 1.25rem;
}
.quiz-info .score{
  color: #FFF;
  font-size: 1.25rem;
}
.options{
  margin-bottom: 1rem;
}
.option{
  padding: 1rem;
  display: block;
  background-color: #271C36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover{
  background-color: #2d213f;
}
.option.correct{
  background-color: #2cce7d;
}
.option.incorrect{
  background-color: #fa1919;
}
.option:last-of-type{
  margin-bottom: 0;
}
.option.disabled{
opacity: 0.5;
}
.option input{
  display: none;
}

button{
  appearance: none;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}
button:disabled{
  opacity: 0.5;
}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
}
p{
  font-size: 1.25rem;
  color: #8F8F8F;
  text-align: center;
}
</style>
