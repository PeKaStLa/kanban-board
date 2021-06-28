<svelte:head>
	<title>Kanban-Board</title>
</svelte:head>

<script lang="ts">
	import Item from './item.svelte';
	import { beforeUpdate, afterUpdate } from 'svelte';
	import { is_empty } from 'svelte/internal';
	

	let divTodo;
	let divProgress;
	let divDone;
	let autoscroll;

	let textfieldTodo: string;
	let textfieldProgress: string;
	let textfieldDone: string;

	let todo: string[] = [];
	let progress: string[] = [];
	let done: string[] = [];

	beforeUpdate(() => {
		autoscroll = divTodo && divTodo.offsetHeight + divTodo.scrollTop > divTodo.scrollHeight - 20;
		autoscroll =
			divProgress &&
			divProgress.offsetHeight + divProgress.scrollTop > divProgress.scrollHeight - 20;
		autoscroll = divDone && divDone.offsetHeight + divDone.scrollTop > divDone.scrollHeight - 20;
	});

	afterUpdate(() => {
		if (autoscroll) divTodo.scrollTo(0, divTodo.scrollHeight);
		if (autoscroll) divProgress.scrollTo(0, divProgress.scrollHeight);
		if (autoscroll) divDone.scrollTo(0, divDone.scrollHeight);
	});

	function addTodo(item: string) {
		if (item != undefined && item != '') {
			todo.push(item);
			todo = todo;
			textfieldTodo = '';
		}
	}

	function addProgress(item: string) {
		if (item != undefined && item != '') {
			progress.push(item);
			progress = progress;
			textfieldProgress = '';
		}
	}

	function addDone(item: string) {
		if (item != undefined && item != '') {
			done.push(item);
			done = done;
			textfieldDone = '';
		}
	}

	const onKeyPressTodo = (e) => {
		if (e.charCode === 13) addTodo(textfieldTodo);
	};

	const onKeyPressProgress = (e) => {
		if (e.charCode === 13) addProgress(textfieldProgress);
	};

	const onKeyPressDone = (e) => {
		if (e.charCode === 13) addDone(textfieldDone);
	};
</script>

<div class="">
	Make reusable columns!
	<div
		class="  px-2  h-5/6   fixed  z-50 
	 rounded-md content-start w-full 
		 grid-cols-3  grid"
	>
		<div
			class="col-span-1 border-t-2 bg-gray-300 border-l-2 border-r-2 
		rounded-t-md border-gray-500 p-1 mr-2"
		>
			<h2 class="text-center text-2xl">ToDo:</h2>
		</div>
		<div
			class="col-span-1 border-t-2 bg-gray-300 border-l-2 border-r-2
			 rounded-t-md border-gray-500 p-1 mr-2"
		>
			<h2 class=" text-center text-2xl">In Progress:</h2>
		</div>
		<div
			class="col-span-1 border-t-2 bg-gray-300  border-l-2 border-r-2 
			rounded-t-md border-gray-500 p-1 "
		>
			<h2 class="text-center  text-2xl">Done:</h2>
		</div>

		<div
			class="p-2 col-span-1 mr-2 bg-gray-300 
 border-l-2 border-r-2 border-gray-500 {is_empty(todo) ? 'rounded-b-md  border-b-2' : ''}"
		>
			<div
				class="text-center  overflow-hidden rounded-md 
border-2  bg-gray-500 col-span-1 border-t-2 border-l-2 border-r-2 
rounded-t-md border-gray-500 p-1 "
			>
				<div class="inline-block   rounded-sm p-2">
					<input
						on:keypress={onKeyPressTodo}
						bind:value={textfieldTodo}
						placeholder="enter an item"
					/>
				</div>
				<div class="inline-block">
					<button class="bg-gray-100 p-2" on:click={() => addTodo(textfieldTodo)}>Add Item</button>
				</div>
			</div>
		</div>
		<div
			class="p-2 col-span-1 mr-2 bg-gray-300 
 border-l-2 border-r-2 border-gray-500 {is_empty(progress) ? 'rounded-b-md  border-b-2' : ''}"
		>
			<div
				class="text-center  overflow-hidden rounded-md 
border-2  bg-gray-500 col-span-1 border-t-2 border-l-2 border-r-2 
rounded-t-md border-gray-500 p-1 "
			>
				<div class="inline-block   rounded-sm p-2">
					<input
						on:keypress={onKeyPressProgress}
						bind:value={textfieldProgress}
						placeholder="enter an item"
					/>
				</div>
				<div class="inline-block">
					<button class="bg-gray-100 p-2" on:click={() => addProgress(textfieldProgress)}
						>Add Item</button
					>
				</div>
			</div>
		</div>

		<div
			class="p-2 col-span-1  bg-gray-300 
 border-l-2 border-r-2 border-gray-500 {is_empty(done) ? 'rounded-b-md  border-b-2' : ''}"
		>
			<div
				class="text-center  overflow-hidden rounded-md 
border-2  bg-gray-500  border-t-2 border-l-2 border-r-2 
rounded-t-md border-gray-500 p-1 "
			>
				<div class="inline-block   rounded-sm p-2">
					<input
						on:keypress={onKeyPressDone}
						bind:value={textfieldDone}
						placeholder="enter an item"
					/>
				</div>
				<div class="inline-block">
					<button class="bg-gray-100 p-2" on:click={() => addDone(textfieldDone)}>Add Item</button>
				</div>
			</div>
		</div>

		<div class="max-h-full 	overflow-y-auto rounded-b-md ">
			<div
				bind:this={divTodo}
				class="  px-1 {is_empty(todo) ? '' : 'border-b-2'}   border-r-2 border-l-2 rounded-b-md  
				bg-gray-300  
			  border-gray-500 mr-2  max-h-full    	overflow-y-auto"
			>
				<div
					class="  rounded-md 
		   col-span-1  "
				>
					{#if !is_empty(todo)}
						{#each todo as item}
							<Item {item} />
						{/each}
					{/if}
				</div>
			</div>
		</div>
		<div class="max-h-full 	overflow-y-auto ">
			<div
				bind:this={divProgress}
				class="  px-1  {is_empty(progress)
					? ''
					: 'border-b-2'}  border-r-2 border-l-2 rounded-b-md  bg-gray-300  
				border-gray-500 mr-2  max-h-full    	overflow-y-auto"
			>
				<div class=" rounded-md   		col-span-1  ">
					{#if !is_empty(progress)}
						{#each progress as item}
							<Item {item} />
						{/each}
					{/if}
				</div>
			</div>
		</div>
		<div class="max-h-full 	overflow-y-auto  ">
			<div
				bind:this={divDone}
				class=" {is_empty(done)
					? ''
					: 'border-b-2'}  px-1   border-r-2 border-l-2 rounded-b-md bg-gray-300  
				border-gray-500   max-h-full     	overflow-y-auto"
			>
				<div class=" rounded-md col-span-1  ">
					{#if !is_empty(done)}
						{#each done as item}
							<Item {item} />
						{/each}
					{/if}
				</div>
			</div>
		</div>
	</div>
</div>

<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>
