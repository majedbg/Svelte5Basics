<script lang='ts'>
  import Header from './Header.svelte';
  let status: 'OPEN' | 'CLOSE' = $state('OPEN');
  
  function toggleStatus() {
    status = status === 'OPEN' ?
    'CLOSE' : 'OPEN'
  };
  
  let name = $state('Scotte');

</script>

<div class="bigContainer">
  <h1>Welcome to Svelte 5</h1>
  <Header name={name} toggle={toggleStatus}/>

<input type="text" bind:value={name} />

  <p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
  
  <p>the store is now {status}</p>
  <button onclick={toggleStatus}>Toggle status</button>
</div>

<style>
  .bigContainer {
    background-color: rgb(34, 19, 82);
    height: 100vh;
  }
  h1, p {
    color: rgb(70, 185, 66);
    font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-weight: 300;
  }
    </style>