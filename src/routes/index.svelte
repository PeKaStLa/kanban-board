<script lang="ts">
	import Item from './item.svelte';
	import Column from './column.svelte';
	import { beforeUpdate, afterUpdate } from 'svelte';
	import { is_empty, select_option } from 'svelte/internal';
	import { onMount } from 'svelte';

	let _DIV_TODO;
	let _DIV_PROGRESS;
	let _DIV_DONE;
	let _AUTOSCROLL;

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


</script>

<svelte:head>
	<title>Kanban-Board</title>
</svelte:head>

<div class="flex pt-14 flex-wrap sm:h-screen w-full bg-cover">
	<Column _TITLE = 'Todo' _URL_PARAM = "todo"/>
	<Column _TITLE = 'In Progress' _URL_PARAM = "progress" />
	<Column _TITLE = 'Done' _URL_PARAM = "done" />
</div>


<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>
