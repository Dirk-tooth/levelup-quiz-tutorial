<script>
  // imports
  import Question from "./question.svelte";

  // js
  let result;

  let quiz = getQuiz();

  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10&category=20&type=multiple");
    const quiz = await res.json();
    return quiz;
  }

  function handleClick() {
    quiz = getQuiz();
  }

</script>

<style>

</style>

<div>
  <button on:click={handleClick}>Get New Questions</button>

  {#await quiz}
    loading...
  {:then data}
    {#each data.results as question}
     <Question question={question} />
    {/each}
  {/await}

</div>
