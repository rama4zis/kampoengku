<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import Navbar from '$lib/components/navbar.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<Navbar />
<div class="mx-auto min-h-screen bg-gradient-to-b from-[#141E30] to-[#243B55] text-white">
	<div class="container mx-auto px-4 py-8">
		{@render children?.()}
	</div>
</div>
