<script>
  // imports
  import Question from "./question.svelte";

  // js
  let activeQuestion = 0;
  let score = 0;
  let result;

  let quiz = getQuiz();

  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10&category=20&type=multiple");
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
  }

  function addToScore() {
    score = score + 1;
  }

</script>

<style>

</style>

<div>
  <button on:click={resetQuiz}>Start New Questions</button>

  <h3>My Score: {score}</h3>
  <h4>Queston #{activeQuestion + 1}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
      <Question {addToScore} {nextQuestion} {question} />
      {/if}
    {/each}
  {/await}

</div>
