<script>
  import { fade, fly } from 'svelte/transition';
  import { createEventDispatcher } from 'svelte';

  let w;

  const dispatch = createEventDispatcher();
</script>

<style lang="scss">
 .modal-bg {
   position: fixed;
   top: 0;
   left: 0;
   right: 0;
   bottom: 0;
   background: rgba(0, 0, 0, 0.8);
   .modal {
     background: white;
     padding: 20px;
     margin: 5vw;
     border-radius: 15px;
   }
  }
</style>

<div class='modal-bg' transition:fade bind:clientWidth={w}>
  <div transition:fly={{ y: 200 }} class="modal">
    <button on:click={ () => dispatch('close') } >
      Close
    </button>
    <div>This div's width is {w}</div>
    <slot />
  </div>
</div>
