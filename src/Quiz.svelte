<script>
// import { get } from "svelte/store";
import { fade, blur, fly, slide, scale } from "svelte/transition";
import {onMount, beforeUpdate, afterUpdate, onDestroy} from 'svelte'
import Question from "./Question.svelte"
import Modal from './Modal.svelte'
import {score} from "./store.js"

let activeQuestion = 0 

let quiz = getQuiz()
let isModalOpen = false

onMount(() => {
    console.log("I mounted");
})
beforeUpdate(() => {
    console.log("before update");

})
afterUpdate(() => {

    console.log("after update");
})
// onDestroy(() => {

// })


async function getQuiz() {
    const res = await fetch('https://opentdb.com/api.php?amount=10&category=12&type=multiple')
    const quiz = await res.json()
    return quiz
}

 

function resetQuiz() {
    isModalOpen = false
    score.set(0)
    activeQuestion = 0
    quiz = getQuiz()
}

function nextQuestion() {
    activeQuestion += 1}

function addToScore() {
    score += 1
}


// Reactive statement
$: if( $score > 0) {  
    isModalOpen = true
}

// Reactive declaration
$: questionNumber = activeQuestion + 1

</script>

<div> 
    <button on:click|once={resetQuiz}>Start New Quiz</button>
 
    <h3>My Score: {$score}</h3>
    <h4>Question #{questionNumber}</h4>
<div class="container">

    {#await quiz}
        Loading...
    {:then data} 
    <!-- <h3>{data.results[0].question}</h3> -->
        
    {#each data.results as question, index}

    {#if index === activeQuestion}
        <div in:fly={{y: 100, x: 100}} out:fly={{y: 100, x: 100}} class="fade-wrapper">
            <Question  {nextQuestion} {question}/>
        </div>
    {/if}
    {/each} 
    {/await}
</div>

</div>
{#if isModalOpen}
<Modal on:close={resetQuiz}>
    <h2 >You won!</h2>
    <p>Congratulations</p>
    <button on:click={resetQuiz}>Start Over</button>
</Modal>
{/if}
  <style>
      .fade-wrapper {
          position:absolute
      }

      .container {
          min-height: 500px;
      }
  </style>