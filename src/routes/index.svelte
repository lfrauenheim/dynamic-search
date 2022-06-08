<script>
  import { fade } from 'svelte/transition';
  let query
  let promise

	async function getData(q) {
    const res = await fetch(`https://jsonplaceholder.typicode.com/photos/${q}`);
    const data = await res.json();
    
    console.log(data)
    
    if (res.ok) {
      return data;
    } 
	}
	
	function handleInput(q) {
    console.log(q)
    promise = getData(q);   
	}

  $: handleInput(query)
</script>



<div>
  <p>Range 1 - 5000</p>
  <form>
    <input type="text" bind:value="{query}">
  </form>
  {#await promise}
    <p>Waiting...</p>
  {:then img}
    <div class="content" in:fade>
      <p>ID: {img.id}</p>
      <p>Title: {img.title}</p>
      <img src="{img.url}" alt="{img.title}">
    </div>
  {/await}
</div>

<style>
  div {
    display: flex;
    flex-flow: column;
    place-items: center
  }
  form {
    border-radius: 1.5rem;
  }
  input {
    text-align: center;
    padding: .2rem;
  }
  p {
    margin: .5rem;
  }
</style>


