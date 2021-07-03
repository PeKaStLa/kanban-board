<script lang="ts">
	import Item from './item.svelte';
	import { beforeUpdate, afterUpdate } from 'svelte';
	import { is_empty, select_option } from 'svelte/internal';
	import { onMount } from 'svelte';

	export let _TITLE = 'default empty';
	export let _URL_PARAM = 'no URL-Param';

	let _URL = 'http://localhost:8000/';
	let _ARR = [];

	let _DIV;
	let _AUTOSCROLL;

	let _TEXTFIELD;

	async function _get_init_items(_URL, _URL_PARAM) {
		const _RES = await fetch(_URL + _URL_PARAM);
		const _RES_JSON = await _RES.json();
		if (_RES.status == 200) {
			return _RES_JSON[0].text;
		}
		if (_RES.status == 400) {
			console.log(_URL_PARAM + ' - status: ', _RES.status, ' - error: ', _RES_JSON);
			return '400_ERROR_NO_INIT_ITEM';
		}
	}

	onMount(async () => {
		_ARR[0] = await _get_init_items(_URL, _URL_PARAM);
		if (_ARR[0] == '400_ERROR_NO_INIT_ITEM') {
			_ARR = [];
		}
	});

	beforeUpdate(() => {
		_AUTOSCROLL = _DIV && _DIV.offsetHeight + _DIV.scrollTop > _DIV.scrollHeight - 20;
	});

	afterUpdate(() => {
		if (_AUTOSCROLL) _DIV.scrollTo(0, _DIV.scrollHeight);
	});

	function _add(_ITEM) {
		if (_ITEM != undefined && _ITEM != '') {
			_ARR = [..._ARR, _ITEM];
			_TEXTFIELD = '';
		}
	}

	const _on_key_press = (e) => {
		if (e.charCode === 13) _add(_TEXTFIELD);
	};
</script>

<div class=" p-1 sm:w-1/3  flex-auto sm:h-35/40 lg:h-36/40	">
	<div
		class="truncate text-white  rounded-t-md 
     py-1 
    bg-gray-800 text-center  text-2xl"
	>
		<h2>{_TITLE}</h2>
	</div>
	{#if !is_empty(_ARR)}
		<div bind:this={_DIV}
			class="bg-gray-300 border-r-2 border-l-2 
            sm:max-h-full sm:overflow-y-auto
        border-gray-200 py-1"
		>
			{#each _ARR as _ITEM}
				<Item {_ITEM} />
			{/each}
		</div>
	{/if}
	<div
		class="text-center  rounded-b-md 
 bg-gray-400  p-1"
	>
		<div class="inline-block   rounded-sm p-1">
			<input
				class="rounded-sm w-full"
				on:keypress={_on_key_press}
				bind:value={_TEXTFIELD}
				placeholder="enter an Item"
			/>
		</div>
		<div class="inline-block">
			<button class="bg-gray-800 text-white rounded-md p-1 m-1 " on:click={() => _add(_TEXTFIELD)}
				>Add Item</button
			>
		</div>
	</div>
</div>
