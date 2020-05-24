<script>
  // imports
  import Quiz from './quiz.svelte';

</script>

<main>
  <h1>Quizit!</h1>
  <Quiz/>
</main>

<style>
  main {
    text-align: center;
    padding: 5vw;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }



  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
