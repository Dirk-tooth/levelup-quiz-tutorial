<script>
  // imports
  import { fly } from 'svelte/transition';
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from 'svelte';
  import Question from './question.svelte';
  import Modal from './modal.svelte';

  // js
  let activeQuestion = 0;
  let score = 0;
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

  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10&category=12&type=multiple");
    const quiz = await res.json();
    return quiz;
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function resetQuiz() {
    score = 0;
    activeQuestion = 0;
    quiz = getQuiz();
    isModalOpen = false;
  }

  function addToScore() {
    score = score + 1;
  }

  // reactive statment
  $: if(score > 1) {
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
</style>

<div>
  <button on:click={resetQuiz}>Start New Questions</button>

  <h3>My Score: {score}</h3>
  <h4>Queston #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
      <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
        <Question {addToScore} {nextQuestion} {question} />
      </div>
      {/if}
    {/each}
  {/await}
</div>

{#if isModalOpen}
  <Modal>
    <h2>You Won!</h2>
    <p>Congradulations!</p>
    <button on:click={resetQuiz}>Start Over</button>
  </Modal>
{/if}
