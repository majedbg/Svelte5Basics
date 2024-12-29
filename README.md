# Svelte5Basics
to learn by doing. Following https://www.youtube.com/watch?v=8DQailPy3q8
## Notes About Svelte: 
### What makes svelte 5 interesting?
 - It's a compiled language - when writing templates, it feels like html is written but it's html and js compiling (no library code shipping)

# Concept list
#### Values - not components
In react, when a state variable changes and causes a rerender the whole component function reruns.
<br>In svelte we track values: The template tag (eg ```{count}``` inside ```<button></button>```) becomes a compiled reactive variable. 
a variable that becomes html - it's not components that are rerendering it's values.

when a variable is defined with ```$state()```, everywhere it is used, it will cause an update.

- svelte does much less at the library level (compared to react) and much more after the compiling into js.

- What's cool about svelte is we can write html, css and js in the same file.
```html
<style>
  button {
    background: red;
  }
  </style>
  ```
will result in compiled css that looks like 
```css
button.svelte-39ontb { background:red;}
```
^scoped class

- svelte outputs a abstract syntax tree (AST) (?)

- Svelte has less framework code. it's like html with javascript sprinkled into it.
- ```npx sv create``` uses svelte kit. it is the app framework for svelte. you don't need svelt kit to write svelte sites but its easiest way to get up and running.
we won't be doing many svelte kit specific things other than use the basic structure.

- In typical svelte files you end up having a ```<script></script>``` tag:  this is where you do imports, javascript, setup state, etc..
```lang="ts"``` in script tag for the file to setup typescript support
- in Svelte, each component is typically a single file (this is diff from other frameworks like react where many components can live on one)
- ^ we can do that with 'Snippets' - a new feature in Svelte 5, but for the most part we are using a single file component. anything in the component is scoped in that component specifically (CSS included).


### props in Svelte: 
usually when you pass in a variable from somewhere into a component. done with ```propName = {propValue}``` in the tag of component:
```html
<Component propName = "propValue" />
```
and accessed from the component file through:
```javascript
<script> 
  let { propName } = $props();
</script>
```
and passed into html like:
```html
<h1> Hello {propName} </h1>
```
it can also take javascript expression just like JSX:
```html
<h1> Hello {propName == 'John'} </h1> //returns true or false and prints that
```
Note: the dollar sign in ```$props()``` indicates it's a 'rune', which is a svelte language feature (indicates something svelte specific and important going on there)
## Giving a prop a type (typescript)

```js
<script>
let { name } : {
  name: string;
} = $props();
</script>
```
variable wise, one can do:
```js
let name2 = "Scott";
```
and use that in the html (eg ``` <h2>{name2}<h2>```)
<br>But in svelte 5, the proper way to do this is to use the rune $state() to make this variable reactive:
```js
let name2 = $state('Scott');
```
### Input values
it's possible to very directly bind the value of an input field to a state variable using ```bind:value``` :
```
<input type="text" bind:value={name2} />
```
and every subsequent component that takes this variable as a prop will update too (no need for onChange function)

--> can also bind a value to an html element! so one can access the element as a variable! a lot more there

### ```onclick``` events
```js
<script>
  let status = $state('OPEN');
  function toggleStatus() {
    status = status==='OPEN' ? 'CLOSED' : 'OPEN';
  }
  </script>

<p>the store is now {status}</p>
  <button onclick={toggleStatus}>Toggle status</button>
```
the ```onclick``` property of a ```<button>``` (html) takes in the function definition (on react it's onClick, in old svelt it was on:click)
<br><br>
# Svelte kit readme:

Everything you need to build a Svelte project, powered by [`sv`](https://github.com/sveltejs/cli).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npx sv create

# create a new project in my-app
npx sv create my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.
