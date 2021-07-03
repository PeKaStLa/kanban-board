<!-- src/routes/__layout.svelte -->
<script lang="ts">
</script>
<!-- pb-14  -->
<div class=" ">
	<nav class="bg-gray-400 fixed w-full justify-center text-center z-50  ">
		<h1 class="text-3xl font-normal w-auto text-black p-2 ">Kanban-Board</h1>
	</nav>
</div>

<slot />

<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>
