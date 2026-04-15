<script lang="ts">
	import { api } from '$lib/api';
	import { onMount } from 'svelte';

	let hubs: any[] = $state([]);
	let error = $state('');

	async function loadHubs() {
		try {
			hubs = await api.getHubs();
		} catch (e: any) {
			error = e.message;
		}
	}

	onMount(loadHubs);
</script>

<h1 class="text-3xl font-bold">Admin Portal</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>

<div class="mt-8 rounded-lg border p-4 shadow-sm">
	<h2 class="text-xl font-semibold">Backend Connection Test</h2>
	<button
		onclick={loadHubs}
		class="mt-2 rounded bg-blue-600 px-4 py-2 text-white hover:bg-blue-700"
	>
		Refresh Hubs
	</button>

	{#if error}
		<p class="mt-2 text-red-500">Error: {error}</p>
	{/if}

	<ul class="mt-4 list-disc pl-5">
		{#each hubs as hub}
			<li><strong>{hub.name}</strong>: {hub.description}</li>
		{:else}
			<li>No hubs found or connecting...</li>
		{/each}
	</ul>
</div>
