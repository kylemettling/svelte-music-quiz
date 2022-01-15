<script>
// import { get } from "svelte/store";
import { fade, blur, fly, slide, scale } from "svelte/transition";
import {onMount} from 'svelte'
import Question from "./Question.svelte"

let activeQuestion = 0
let score = 0

let quiz = getQuiz()
 

async function getQuiz() {
    const res = await fetch('https://opentdb.com/api.php?amount=10&category=12&type=multiple')
    const quiz = await res.json()
    return quiz
}

 

function resetQuiz() {
    score = 0
    activeQuestion = 0
    quiz = getQuiz()
}

function nextQuestion() {
    activeQuestion += 1}

function addToScore() {
    score += 1
}


// Reactive statement
$: if( score > 1) {
    alert("You won!")
    resetQuiz()
}

// Reactive declaration
$: questionNumber = activeQuestion + 1

</script>

<div> 
    <button on:click={resetQuiz}>Start New Quiz</button>
 
    <h3>My Score: {score}</h3>
    <h4>Question #{questionNumber}</h4>

    {#await quiz}
        Loading...
    {:then data} 
    <!-- <h3>{data.results[0].question}</h3> -->
        
    {#each data.results as question, index}

    {#if index === activeQuestion}
        <div in:fly={{y: 100, x: 100}} out:fly={{y: 100, x: 100}} class="fade-wrapper">
            <Question {addToScore} {nextQuestion} {question}/>
        </div>
    {/if}
    {/each} 
    {/await}

</div>
  <style>
      .fade-wrapper {
          position:absolute
      }
  </style>