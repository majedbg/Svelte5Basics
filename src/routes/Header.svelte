<script lang="ts">
  let { name }: {
    name: string;
  } = $props()

  let middleName = 'Scott';
  let name2 = $state('Skott');
</script>

<h1>Hello from Header, {name +' '+ middleName}</h1>
<h2>{middleName.replaceAll('t','d')}</h2>
<h2>Reactive name: {name2}</h2>

<style>
  h1 {
    color: rgb(195, 244, 89);
  }
  h2 {
    color: red;
  }
</style>