<script>
  // props
  export let question;
  export let nextQuestion;
  export let addToScore;

  // js

  let isAnswered = false;
  let isCorrect;

  let answers = question.incorrect_answers.map(answer => {
    return {
      answer,
      correct: false
    }
  });

  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true
    }
  ]

  shuffle(allAnswers);

  function shuffle(array) {
    array.sort(() => Math.random() - 0.5)
  }

  function checkQuestion(correct) {
    if(!isAnswered) {
      isAnswered = true;
      isCorrect = correct;
      if(correct) {
        addToScore();
      }
    }
  }
</script>

<style>
  h5 {
    color: blue;
  }

  h5.isCorrect {
    color: green;
  }

  h5.wrong {
    color: red;
  }
</style>

<h3>{@html question.question}</h3>

{#if isAnswered}
  <h5 class:isCorrect class:wrong={!isCorrect}>
    {#if isCorrect}
    You got it right!
    {:else}
    You goofed up :(
    {/if}
  </h5>
{/if}

{#each allAnswers as answer}
  <button disabled={isAnswered} on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}>Next Question</button>
  </div>
{/if}
