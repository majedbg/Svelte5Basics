<script lang='ts'>
  import Header from './Header.svelte';
  let formState = $state({
    name: "",
    birthday: "",
    step: 0,
    error: ''
  }); //state for a form
</script>

<main>
  <p>Step: {formState.step + 1}</p>
  {#if formState.error}
  <p class="error">{formState.error}</p>
  {/if}
  {#if formState.birthday === '' }
  <div>
    <label for="bday"> Your Birthday</label>
    <input type="date" id="bday" bind:value={formState.birthday}>
  </div>
  <button onclick={ () => {
    if(formState.name !== "") {
      formState.step += 1;
      formState.error = "";
    } else {
      formState.error = "Empty name field"
    }
  }}>Next</button>
  {:else }
  <h2>birthday: {formState.birthday}</h2>
{/if}
  
</main>