<script>
  // imports
  import { fly } from 'svelte/transition';
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from 'svelte';
  import Question from './question.svelte';
  import Modal from './modal.svelte';

  import { score } from './store.js'

  // js
  let activeQuestion = 0;
  let result;
  let isModalOpen = false;

  let quiz = getQuiz();


  // life cycle methods
  onMount(() => {
    console.log("I mounted");
  })

  beforeUpdate(() => {
    console.log("I'm about to update");
  })

  afterUpdate(() => {
    console.log("I'm done updating");
  })

  onDestroy(() => {
    console.log("I'm gone");
  })

  // https://opentdb.com/api_config.php
  // ^ url for more trivia
  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10&category=12&type=multiple");
    const quiz = await res.json();
    return quiz;
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function resetQuiz() {
    score.set(0);
    activeQuestion = 0;
    quiz = getQuiz();
    isModalOpen = false;
  }

  // reactive statment
  $: if($score > 0) {
    isModalOpen = true;
  }

  // reacctive decloration
  $: questionNumber = activeQuestion + 1

</script>

<style>
 .fade-wrapper {
   position: absolute;
   width: 90vw;
   padding-left: 5vw;
   left: 0;
 }

 footer {
   margin-top: 30vh;
   bottom: 0;
   color: lightgray;
   font-style: italic;
 }
</style>

<div>
  <button on:click={resetQuiz}>Start New Questions</button>

  <h3>Your Score: {$score}</h3>
  <h4>Queston #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
      <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
        <Question {nextQuestion} {question} />
      </div>
      {/if}
    {/each}
  {/await}
</div>

<footer>powered by https://opentdb.com/api_config.php</footer>


{#if isModalOpen}
  <Modal on:close={resetQuiz}>
    <h2>You Won!</h2>
    <p>Congradulations!</p>
    <button on:click={resetQuiz}>Start Over</button>
  </Modal>
{/if}
