<script lang="ts">
	import { slide } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
	import { onMount } from 'svelte';

	interface Service {
		id: number;
		title: string;
		requirements: string[];
	}

	let services: Service[] = [];
	let openItems: boolean[] = [];

	function toggleItem(index: number) {
		openItems[index] = !openItems[index];
	}

	onMount(async () => {
		try {
			const response = await fetch('/data/layanan.json');
			const data = await response.json();
			services = data.services;
			openItems = new Array(services.length).fill(false);
		} catch (error) {
			console.error('Error loading services data:', error);
		}
	});
</script>

<svelte:head>
	<title>Layanan - Kampoeng Ku</title>
	<meta
		name="description"
		content="Daftar layanan administrasi dan pelayanan publik yang tersedia"
	/>
</svelte:head>

<div class="mx-auto max-w-4xl p-6">
	<div class="mb-8 text-center">
		<h1 class="mb-4 text-3xl font-bold text-gray-200">Layanan Administrasi</h1>
		<p class="text-lg text-gray-600">Daftar layanan dan persyaratan yang diperlukan</p>
	</div>

	<div class="space-y-4">
		{#each services as service, index}
			<div class="rounded-lg border border-gray-200 shadow-sm">
				<button
					class="w-full rounded-t-lg bg-gray-50 px-6 py-4 text-left transition-colors duration-200 hover:bg-gray-100 focus:ring-2 focus:ring-blue-500 focus:outline-none"
					onclick={() => toggleItem(index)}
				>
					<div class="flex items-center justify-between">
						<span class="text-lg font-semibold text-gray-900">{service.title}</span>
						<svg
							class="h-5 w-5 transform text-gray-500 transition-transform duration-200 {openItems[
								index
							]
								? 'rotate-180'
								: ''}"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M19 9l-7 7-7-7"
							></path>
						</svg>
					</div>
				</button>
				{#if openItems[index]}
					<div
						class="overflow-hidden border-t border-gray-200 bg-white px-6 py-4"
						transition:slide={{ duration: 300, easing: quintOut }}
					>
						<h3 class="mb-3 font-semibold text-gray-800">Persyaratan:</h3>
						<ul class="space-y-2">
							{#each service.requirements as requirement}
								<li class="flex items-start">
									<span
										class="mt-2 mr-3 inline-block h-2 w-2 flex-shrink-0 rounded-full bg-blue-500"
									></span>
									<span class="text-gray-700">{requirement}</span>
								</li>
							{/each}
						</ul>
					</div>
				{/if}
			</div>
		{/each}

		{#if services.length === 0}
			<div class="py-12 text-center">
				<div
					class="mb-4 inline-flex h-16 w-16 items-center justify-center rounded-full bg-gray-100"
				>
					<svg class="h-8 w-8 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"
						></path>
					</svg>
				</div>
				<p class="text-gray-500">Memuat data layanan...</p>
			</div>
		{/if}

        <div class="mt-8 text-center text-gray-300">
            <p>Untuk informasi lebih lanjut, hubungi Ketua RT:</p>
            <p class="mt-2 font-semibold">Bp. TUKIRAN</p>
            <p>Telp / WA: <a href="https://wa.me/62818509506" class="text-blue-500">62818509506</a></p>
        </div>
	</div>
</div>
