# Svelte5Basics
to learn by doing. Following https://www.youtube.com/watch?v=8DQailPy3q8
<br> Note that all the below notes are transcribing, paraphrasing or reorganizing information from the above tutorial.
# What makes Svelte different
### A compiled language
No library code shipping - Svelte has less framework code. <br> It does much less at the library level (compared to react) and much more after the compiling into js. 
- It outputs a abstract syntax tree (AST) (?)

### We can write html, css and js in the same file.
It's like html with javascript sprinkled into it:

-  The ```<script></script>``` tag is where imports, javascript, and states are setup, etc..
- after that, regular html is written
- after that, styling:
```html
<style>
  button {  background: red; }
</style>
  ```

### Values - not components
 A variable defined with ```$state()``` will cause an update wherever it is used
- In react, when a state variable changes and causes a rerender the whole component function reruns.
- In Svelte, values are tracked: the template tag (eg ```{count}``` inside ```<button></button>```) becomes a compiled reactive variable. 
a variable that becomes html - it's not components that are rerendering it's values.

- ```npx sv create``` uses svelte kit. it is the app framework for svelte. you don't need svelt kit to write svelte sites but its easiest way to get up and running.
we won't be doing many svelte kit specific things other than use the basic structure.

```lang="ts"``` in script tag for the file to setup typescript support
- in Svelte, each component is typically a single file (this is diff from other frameworks like react where many components can live on one)
- ^ we can do that with 'Snippets' - a new feature in Svelte 5, but for the most part we are using a single file component. anything in the component is scoped in that component specifically (CSS included).


# props, ```$props()```: 
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

## Defining a prop's type (Typescript)
semi colon after destructuring the props and nest prop label and type in curly brackets (usual Ts syntax - note it's not an object)

```js
<script>
let { name , clickFunc } : {
  name: string;
  clickFunc: event(MouseEvent) => void; //if passed in to 'onclick'
} = $props();
</script>
```

## optional props:
with Typescript:
```ts
let {
  name,
  optional_prop
}: {
  name : string;
  optional_prop?: string //the question mark tells Ts it might not appear in props
} = $props();
```
without Typescript:
```ts
let {
  name,
  optional_prop = null; //default value
} = $props()
```
## Variables and ```$state``` , ```$derived```
variable wise, one can do:
```js
let name2 = "Scott";
```
and use that in the html (eg ``` <h2>{name2}<h2>```)

### ```$state```
<br>But in svelte 5, the proper way to do this is to use the rune $state() to make this variable reactive:
```js
let name2 = $state('Scott');
```
### ```$derived```
If you have a value that depends on another. <br> eg shopping cart - there are items, as well as a total. <br>
the total sum depends on the items.
this can look like:
```js
let cart = $state(
    [
      {item:'potato', price: 3},
      {item: 'eggs', price: 4}]
      );
let total = $derived({
  cart.reduce(sum,item) => sum + item.price
})
```
so in a way derived will store a transformation and the value that comes out of it.
## Binding values from a ```<input>``` to a variable
it's possible to very directly bind the value of an input field to a state variable using ```bind:value``` :
```
<input type="text" bind:value={name2} />
```
and every subsequent component that takes this variable as a prop will update too (no need for onChange function)

--> can also bind a value to an html element! so one can access the element as a variable! a lot more there


# Complex State
A way to remain organized with states and functions - like with declaring classes that have their own method - is possible. let's say we want to create a way to get a value, increment it, and set it to a certain number, all within what's allowed in Svelte:
```ts
export function createState() {
  let value = $state(0); //initialize as 0

  function increment() { //function definition
    value ++;
  }

  return {
    get value() { //svelte thing to get most recent value of state. simple method. 
      return value;
    }
    set value(newVal) { //a way to allow manually setting value
      value = newVal;
    }
    increment, //expose the function (make it a method)
  }
}
```
now import this to a component and begin using it:
```ts
 <script>
 import {createState} from './fileName.svelte';
 const myState = createState();
 //this now gives us access to doing things to myState:
 </script>

 <h1>{myState.value}<h1> //the get function
 <button onclick={myState.increment}>Increment</button> //the increment function
 <button onclick={() => {myState.value = 10} }>set Val to 10</button> //the set function allows this
```
This all allows ways to interact with a state.

## Doing complex state with classes
This is similar to above but a class so it is neater to write  and allows more:
```ts
export class classState {
  value = $state(0);  //creates it

  increment() { //method
    this.value += 1; //can use 'this'
  }
}
```
Though authoring this is much neater, one small disadvantage is to have to bind it to onclick differently:
```ts
<script>
  import {classState} from './fileName.svelte';
  const myClassState = new classState();
</script>

<button onclick={
  () => myClassState.increment();
  }
> increment </button>
```

# Events, Conditionals and Logic

## ```onclick``` events
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

## Template conditionals and logic

outside the script tags - in between the html tags - one can use ```{#if}``` to begin a section of conditionally rendered html and ```{/if}``` to close it. can also add an ```{:else}```:
```html
{#if count > 0 } 
<p>Count no longer empty</p>
{:else}
<p>Count is empty, add something<p>
<button onclick={()=> count+= 1}></button>
{/if}
```
can also do ```{:else if xyz === 1 }``` (chain conditional logic)
<br>
Note: the backslash in the curly brackets ```{/}``` is common in Svelte - it's similar to how html has closing tags but for Svelte.

## Loops and ```{#each}{/each}```
The ```#each``` in svelte replaces what we usually do with a for loop, map or for each.
e.g. if we have an array:
```ts
const Questions = [ //defined in script tag
  {text: 'question1',  id: '3'},
  {text: 'question2',  id: '4'},
  {text: 'question3',  id: '6'},
]
```
and want to loop through it for rendering html, we can do <br>
```{#each arrayName as arrayValue}``` and choose what to do for each value:
```ts
{#each Questions as question (question.id) } //each loop opening tag
  {question.id} //will render id string
  {@render formStep({question.text})}
{/each} 
//^remember to close it
```
Note the (question.id) is a keyblock that helps the renderer but is not necessary.
<br><br>
 can also destructure the array value properties:
```ts
{#each Questions as {text, id} (id) } 
  {id} //will reference what used to be question.id
  {@render formStep({text})}
{/each}
```
## Effects/side effects (like react's ```useEffect()```)
effects or side-effects just mean "this code runs when something else happens".
<br> slightly more verbose way: "When some data changed or a component is mounted etc, this code will run."
<br>
in Svelte 5, we use ```$effect()``` which takes in a function:
### onMount
```ts
$effect(() => { //by default will run onMount once
  console.log('on mounted'); 
})
```
### onUnmounted, destroyed or before effect Re-runs
```ts
$effect(() => { //by default will run onMount once
  console.log('on mounted'); 
  return () => {
    console.log('post change');
  }
})
```
### on a variable change
```ts
//will rerun when formState.step has changed
$effect( () => { //each time the formStep is run
  console.log('formstate', formState.step);
})
```
This is different from react where the dependency needs to be explicitly placed in another parameter at the end.
<br> and this will happen before it reruns:
```ts
//will rerun when formState.step has changed
$effect( () => { 
  return () => { //before it reruns
    console.log('before formstate reruns', formState.step); //returns old step
  }
})
```
Warning: DON'T create state based off other state inside ```$effect```. Simply use ```$derived``` instead.
## ```$inspect```
use inspect to check on variables in the console - like Svelte's console log, will log whenever an initialization or change happens, eg:
```ts
$inspect(formState.step); 
//logs 'init 0', then 'update 1' when step is incremented
```

# Snippets

A snippet allows to define a reusable chunk of code inside a Svelte template
## ```#snippet```
here a snippet is defined:
```html
{#snippet formStep({ question, id, type} : {question: string; id: string; type: string})}
  <article>
    <div>
      <label for={id}>{question}</label>
      <input {type} {id} bind:value={formState[id]}
      />
    </div>
  </article>
{/snippet}
```
It can now be called in the html section just like a function (eg how react works). However it needs to be run with ```{@render snippetName}```
## ```@render```
In the section of the Svelte file that is rendering html, the snippet can be rendered with ```@render```:
```html
<main>
  {@render formStep({
    question: "What's your name", 
    id: 'name', 
    type: 'text'
    })}
</main>
```
Note the snippet will have access to all variables within the scope of this component and doesn't need its own props.

## Why use a snippet?
- can make a file easier to read
- can be a way to prototype something that might become it's own file later (with props and all)
- can help render children in the component it is in. For example this is usually done like this with ```@render```:
```ts
//1_it's possible to use a component to wrap other things
<Header name={formState.name}>
  <div>
    <p>I'm the Header's child</p>
  </div>
</Header>
```
```ts
//2_ What is wrapped is now accessible through the 'children' prop inside the component file:
  <script lang='ts'>
  import type { Snippet } from 'svelte';
  let { 
    name,
    children //contains whatever was between <Header></Header>
    } : {
      name: string;
      children: Snippet
    } = $props();
  </script>
```
```ts
//now render children with file html
<div>
{@render children()} //here
</div>
```
Important note: powerful because children passed in this way can pass information upwards, like what prop drilling achieves. eg if one of the children is a snippet or function that uses variables from parent component.



# Styling in Svelte
## local scoping
Global styling still works (more later). But the powerful additional that Svelte adds is that the ```<style></style>``` tags in a single file component targets the styles in that file only (not parents or children). This helps with scoping styling locally.
eg:
```html
<p class="error">Error message</p>

<style>
p {
  color: red;
}
</style>
```
The way it works is Svelte will create a class just for this element behind the scenes eg '```.p.svelte-25onta1```'

## global scoping
to have styling affect elements outside the component, we can use the ```:global()``` selector function:
```html
<style>
  :global(p) {
    color: red;
  }
</style>
```
now any paragraph will receive this style

## Animations
Can use ```transition:``` by setting properties to html elements, eg:
```ts
<div transition:fly={{ x: 200, duration: 200, opacity: 0}}>
  //need to import: import 'Fly' from 'svelte/transition';
```
Can also do ```in:``` and ```out:``` :
```ts
<div in:fly = {{ x: 200, duration: 200, opacity: 0, delay: 200}}
    out:fly = {{ x: -200,duration: 200, opacity: 0}}
>
  
```
There are other transitions in addition to ```fly```, like ```fade```, ```blur```, ```slide```, ```scale```, ```draw``` and ```crossfade```.<br>
There is a lot more animation things like action, motion, etc. <br>See eg https://svelte.dev/docs/svelte/svelte-motion
