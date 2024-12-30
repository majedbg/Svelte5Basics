<script lang="ts">
  let { name , toggle }: {
    name: string;
    toggle: (event: MouseEvent) => void;
  } = $props()

  let middleName = 'Scott';
  let name2 = $state('Skott');
</script>

<h1>Hello from Header, {name +' '+ middleName}</h1>
<h2>{name.replaceAll('t','d')}</h2>
<h2>Reactive name: {name2}</h2>
<button onclick={toggle}>header toggle status</button>
<style>
  h1 {
    color: rgb(195, 244, 89);
  }
  h2 {
    color: red;
  }
</style>