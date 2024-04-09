<script>
	import '../style.css';
	import {writable} from 'svelte/store';
     let todoItem = '';
	let storedList;
	let todoList = writable([]);
	
	if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
         storedList = localStorage.getItem('storedList');
	    if(storedList) {
		$todoList = (JSON.parse(storedList));
	    }
	}

	function updateList() {
		return storedList = localStorage.setItem('storedList', JSON.stringify($todoList));
	}

	//$: isDone = $todoList.filter(item => item.done);
	//ajhsfhavsfhvsaldf
	function addToArray() {
		if (todoItem == '') {
			return;
		}
		$todoList = [...$todoList, {
			text: todoItem,
			done: false
		}];
		//console.log(todoList);
		updateList();
		todoItem = '';
	}
	function removeThis(index) {
		$todoList.splice(index, 1);
		$todoList = $todoList;
		updateList();
	}
	function clearDone (){

	}

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app"/>
</svelte:head>
<header>
	<h1>Simple</h1>
</header>
<h1>Task</h1>

<form on:submit|preventDefault={addToArray}>
	<input type="text" bind:value={todoItem}>
	<button type="submit">Add</button>
</form>

<ul>
	{#each $todoList as item, index}
	     <li>
			<input type="checkbox" bind:checked={item.done} on:change={updateList}>

			<span class:done={item.done}>{item.text}</span>
			<span on:click={() => removeThis(index)} class="remove" role="button" tabindex="0">&times;</span>
		</li>
	{/each}
</ul>

<style>
	ul {
		list-style: none;
	}
	li {
		font-size: 2rem;
	}
	.done{
		color: #999;
		text-decoration: line-through;
	}
	.remove{
		color: darkred;
		cursor: pointer;
	}
</style>