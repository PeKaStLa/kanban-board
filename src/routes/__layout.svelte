<!-- src/routes/__layout.svelte -->
<script lang="ts">
</script>

<div class=" pb-16 ">
	<nav class="bg-gray-800 fixed w-full justify-center text-center z-50  ">
		<h1 class="text-3xl font-normal w-auto text-white p-2">Kanban-Board</h1>
	</nav>
</div>

<slot />

<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>
