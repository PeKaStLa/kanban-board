<script lang="ts">
	import Column from './column.svelte';
	import { beforeUpdate, afterUpdate } from 'svelte';
	import { is_empty, select_option } from 'svelte/internal';
	import { onMount } from 'svelte';

</script>

<svelte:head>
	<title>Kanban-Board</title>
</svelte:head>

<div class="flex pt-14 flex-wrap sm:h-screen w-full bg-cover">
	<Column _TITLE = 'ToDo' _URL_PARAM = "todo"/>
	<Column _TITLE = 'In Progress' _URL_PARAM = "progress" />
	<Column _TITLE = 'Done' _URL_PARAM = "done" />
</div>


<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>
