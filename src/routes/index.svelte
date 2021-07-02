<script lang="ts">
	import Item from './item.svelte';
	import { beforeUpdate, afterUpdate } from 'svelte';
	import { is_empty, select_option } from 'svelte/internal';
	import { onMount } from 'svelte';

	let _DIV_TODO;
	let _DIV_PROGRESS;
	let _DIV_DONE;
	let _AUTOSCROLL;

	let _TEXTFIELD_TODO;
	let _TEXTFIELD_PROGRESS;
	let _TEXTFIELD_DONE;

	let _TODO = [];
	let _PROGRESS = [];
	let _DONE = [];

	async function _get_init_items(_ARR, _URL, _URL_PARAM) {
		const _RES = await fetch(_URL + _URL_PARAM);
		if (_RES.status == 200){
		const _RES_JSON = await _RES.json();
		console.log(_RES_JSON[0].text)

		_ARR = [..._ARR, _RES_JSON[0].text];
		
		//_ARR.push(_RES_JSON[0].text);
		//_ARR = _ARR;

		console.log(_URL_PARAM + " status = 200")
		}
		if (_RES.status == 400){
		console.log(_URL_PARAM +  " status = 400")
		}
		_ARR = _ARR;
	}

	onMount(async () => {
	
	/*	
	_get_init_items(_TODO, 'http://peterstadler.com:7000/', 'todo') 
	_get_init_items(_PROGRESS, 'http://peterstadler.com:7000/', 'progress') 
	_get_init_items(_DONE, 'http://localhost:8000/', 'done') 

	_get_init_items(_TODO, 'http://peterstadler.com:7000/', 'todo') 
	_get_init_items(_PROGRESS, 'http://peterstadler.com:7000/', 'progress') 
	_get_init_items(_DONE, 'http://localhost:8000/', 'done') 
	_get_init_items(_DONE, 'http://localhost:8000/', 'done') 
*/
		const _RES_TODO = await fetch('http://peterstadler.com:7000/todo');
		if (_RES_TODO.status == 200){
		const _RES_TODO_JSON = await _RES_TODO.json();
		console.log(_RES_TODO_JSON[0].text)
	_TODO = [..._TODO, _RES_TODO_JSON[0].text];
		console.log("todo status = 200")
		}
		if (_RES_TODO.status == 400){
console.log("todo status = 400")
		}


		const _RES_PROGRESS = await fetch('http://peterstadler.com:7000/progress');
		if (_RES_PROGRESS.status == 200){
		const _RES_PROGRESS_JSON = await _RES_PROGRESS.json();
		console.log(_RES_PROGRESS_JSON[0].text)
		_PROGRESS = [..._PROGRESS, _RES_PROGRESS_JSON[0].text];
		console.log("progress status = 200")
		}
		if (_RES_PROGRESS.status == 400){
console.log("progress status = 400")
		}

		//const _RES_DONE = await fetch('http://peterstadler.com:7000/done');
		const _RES_DONE = await fetch('http://localhost:8000/done');
		if (_RES_DONE.status == 200){
		const _RES_DONE_JSON = await _RES_DONE.json();
		console.log(_RES_DONE_JSON[0].text)
		_DONE = [..._DONE, _RES_DONE_JSON[0].text];
		console.log("done status = 200")
		}
		if (_RES_DONE.status == 400){
console.log("done status = 400")
		}
	
		console.log(_TODO)
		console.log(_PROGRESS)
		console.log(_DONE)
	});

	beforeUpdate(() => {
		_AUTOSCROLL =
			_DIV_TODO && _DIV_TODO.offsetHeight + _DIV_TODO.scrollTop > _DIV_TODO.scrollHeight - 20;
		_AUTOSCROLL =
			_DIV_PROGRESS &&
			_DIV_PROGRESS.offsetHeight + _DIV_PROGRESS.scrollTop > _DIV_PROGRESS.scrollHeight - 20;
		_AUTOSCROLL =
			_DIV_DONE && _DIV_DONE.offsetHeight + _DIV_DONE.scrollTop > _DIV_DONE.scrollHeight - 20;
	});

	afterUpdate(() => {
		if (_AUTOSCROLL) _DIV_TODO.scrollTo(0, _DIV_TODO.scrollHeight);
		if (_AUTOSCROLL) _DIV_PROGRESS.scrollTo(0, _DIV_PROGRESS.scrollHeight);
		if (_AUTOSCROLL) _DIV_DONE.scrollTo(0, _DIV_DONE.scrollHeight);
	});

	function _add_todo(_ITEM) {
		if (_ITEM != undefined && _ITEM != '') {
			_TODO.push(_ITEM);
			_TODO = _TODO;
			_TEXTFIELD_TODO = '';
		}
	}

	function _add_progress(_ITEM) {
		if (_ITEM != undefined && _ITEM != '') {
			_PROGRESS.push(_ITEM);
			_PROGRESS = _PROGRESS;
			_TEXTFIELD_PROGRESS = '';
		}
	}

	function _add_done(_ITEM) {
		if (_ITEM != undefined && _ITEM != '') {
			_DONE.push(_ITEM);
			_DONE = _DONE;
			_TEXTFIELD_DONE = '';
		}
	}

	const _on_key_press_todo = (e) => {
		if (e.charCode === 13) _add_todo(_TEXTFIELD_TODO);
	};

	const _on_key_press_progress = (e) => {
		if (e.charCode === 13) _add_progress(_TEXTFIELD_PROGRESS);
	};

	const _on_key_press_done = (e) => {
		if (e.charCode === 13) _add_done(_TEXTFIELD_DONE);
	};
</script>

<svelte:head>
	<title>Kanban-Board</title>
</svelte:head>

<div class="">
	<!--Make reusable columns!-->
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
 border-l-2 border-r-2 border-gray-500 {is_empty(_TODO) ? 'rounded-b-md  border-b-2' : ''}"
		>
			<div
				class="text-center  overflow-hidden rounded-md 
border-2  bg-gray-500 col-span-1 border-t-2 border-l-2 border-r-2 
rounded-t-md border-gray-500 p-1 "
			>
				<div class="inline-block   rounded-sm p-2">
					<input
						on:keypress={_on_key_press_todo}
						bind:value={_TEXTFIELD_TODO}
						placeholder="enter an Item"
					/>
				</div>
				<div class="inline-block">
					<button class="bg-gray-100 p-2" on:click={() => _add_todo(_TEXTFIELD_TODO)}
						>Add Item</button
					>
				</div>
			</div>
		</div>
		<div
			class="p-2 col-span-1 mr-2 bg-gray-300 
 border-l-2 border-r-2 border-gray-500 {is_empty(_PROGRESS) ? 'rounded-b-md  border-b-2' : ''}"
		>
			<div
				class="text-center  overflow-hidden rounded-md 
border-2  bg-gray-500 col-span-1 border-t-2 border-l-2 border-r-2 
rounded-t-md border-gray-500 p-1 "
			>
				<div class="inline-block   rounded-sm p-2">
					<input
						on:keypress={_on_key_press_progress}
						bind:value={_TEXTFIELD_PROGRESS}
						placeholder="enter an Item"
					/>
				</div>
				<div class="inline-block">
					<button class="bg-gray-100 p-2" on:click={() => _add_progress(_TEXTFIELD_PROGRESS)}
						>Add Item</button
					>
				</div>
			</div>
		</div>

		<div
			class="p-2 col-span-1  bg-gray-300 
 border-l-2 border-r-2 border-gray-500 {is_empty(_DONE) ? 'rounded-b-md  border-b-2' : ''}"
		>
			<div
				class="text-center  overflow-hidden rounded-md 
border-2  bg-gray-500  border-t-2 border-l-2 border-r-2 
rounded-t-md border-gray-500 p-1 "
			>
				<div class="inline-block   rounded-sm p-2">
					<input
						on:keypress={_on_key_press_done}
						bind:value={_TEXTFIELD_DONE}
						placeholder="enter an Item"
					/>
				</div>
				<div class="inline-block">
					<button class="bg-gray-100 p-2" on:click={() => _add_done(_TEXTFIELD_DONE)}
						>Add Item</button
					>
				</div>
			</div>
		</div>

		<div class="max-h-full 	overflow-y-auto rounded-b-md ">
			<div
				bind:this={_DIV_TODO}
				class="  px-1 {is_empty(_TODO) ? '' : 'border-b-2'}   border-r-2 border-l-2 rounded-b-md  
				bg-gray-300  
			  border-gray-500 mr-2  max-h-full    	overflow-y-auto"
			>
				<div class="  rounded-md 		   col-span-1  ">
					{#if !is_empty(_TODO)}
						{#each _TODO as _ITEM}
							<Item {_ITEM} />
						{/each}
					{/if}
				</div>
			</div>
		</div>
		<div class="max-h-full 	overflow-y-auto ">
			<div
				bind:this={_DIV_PROGRESS}
				class="  px-1  {is_empty(_PROGRESS)
					? ''
					: 'border-b-2'}  border-r-2 border-l-2 rounded-b-md  bg-gray-300  
				border-gray-500 mr-2  max-h-full    	overflow-y-auto"
			>
				<div class=" rounded-md   		col-span-1  ">
					{#if !is_empty(_PROGRESS)}
						{#each _PROGRESS as _ITEM}
							<Item {_ITEM} />
						{/each}
					{/if}
				</div>
			</div>
		</div>
		<div class="max-h-full 	overflow-y-auto  ">
			<div
				bind:this={_DIV_DONE}
				class=" {is_empty(_DONE)
					? ''
					: 'border-b-2'}  px-1   border-r-2 border-l-2 rounded-b-md bg-gray-300  
				border-gray-500   max-h-full     	overflow-y-auto"
			>
				<div class=" rounded-md col-span-1  ">
					{#if !is_empty(_DONE)}
						{#each _DONE as _ITEM}
							<Item {_ITEM} />
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
