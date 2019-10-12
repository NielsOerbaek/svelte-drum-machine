<script>
  import {createEventDispatcher} from 'svelte';
  const dispatch = createEventDispatcher();

  export let state;
  export let sounds;
  export let highlight;
  export let height;

  $: console.debug("State:", state)
</script>


<style>
  .drum-field {
    border: 1px solid black;
    border-radius: 10px;
    box-shadow: 5px 5px 0 2px rgba(150,150,150,0.8);
    display: grid;
    grid-template-columns: 1fr  1fr;
    grid-template-rows: auto;
    place-items: center;
    background: white;
    transition: all 100ms linear;
  }
  .drum-field.highlight-true {
    background: yellowgreen;
  }

  .sound-button {
    width:100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  h6 {
    text-transform: capitalize;
  }

  .on-label {
    width: 80%;
    height: 50%;
    background: black;
    box-shadow: 5px 5px 0 0 #555;
    transform: none;
    transition: 300ms all ease;
  }
  .on-label:active {
    box-shadow: 0px 0px 0 0 #555;
    transform: translate(5px,5px);
  }
  .on-label.on-true {
    background: orangered;
  }
</style>

<div class={'drum-field highlight-' + highlight} style={"height:"+(95/height)+"vh"}>
  {#each sounds as sound }
    <div class="sound-button">
      <h6>{sound}</h6>
      <div
        class={"on-label on-" + state[sound]}
        on:click={() => dispatch("click",{sound: sound})}
      />
    </div>
  {/each}
</div>
