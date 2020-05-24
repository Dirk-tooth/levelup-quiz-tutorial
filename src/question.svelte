<script>
  // imports
  import { score } from './store.js'
  // props
  export let question;
  export let nextQuestion;

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
        score.update(n => n + 1)
      }
    }
  }
</script>

<style>
  h5 {
    color: red;
  }

  h5.isCorrect {
    color: green;
  }

  button.correct {
    color: green;
  }

  button.incorrect {
    color: red;
  }

  button.unAnswered {
    color: black;
  }

</style>

<h3>{@html question.question}</h3>

{#if isAnswered}
  <h5 class:isCorrect>
    {#if isCorrect}
    You got it right!
    {:else}
    You goofed up :(
    {/if}
  </h5>
{/if}

{#each allAnswers as answer}
  <button class={!isAnswered ? 'unAnswered' : answer.correct ? 'correct' : 'incorrect'} disabled={isAnswered} on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}>Next Question</button>
  </div>
{/if}
