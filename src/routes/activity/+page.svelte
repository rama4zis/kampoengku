<script lang="ts">
	import { onMount } from 'svelte';

	interface ActivityPost {
		id: string;
		imageUrl: string;
		caption: string;
		date: string;
		year: number;
		// likes?: number;
		location?: string;
	}

	let activityPosts: ActivityPost[] = [];
	let groupedByYear: { [year: number]: ActivityPost[] } = {};
	let availableYears: number[] = [];
	let selectedYear: number | 'all' = 'all';
	let isLoading = true;

	onMount(async () => {
		try {
			// Load activity posts from JSON file
			const response = await fetch('/data/activity.json');
			if (!response.ok) {
				throw new Error('Failed to load activity data');
			}
			activityPosts = await response.json();

			// Group posts by year
			groupedByYear = activityPosts.reduce(
				(acc, post) => {
					if (!acc[post.year]) {
						acc[post.year] = [];
					}
					acc[post.year].push(post);
					return acc;
				},
				{} as { [year: number]: ActivityPost[] }
			);

			// Sort posts within each year by date (newest first)
			Object.keys(groupedByYear).forEach((year) => {
				groupedByYear[parseInt(year)].sort(
					(a, b) => new Date(b.date).getTime() - new Date(a.date).getTime()
				);
			});

			// Get available years and sort descending
			availableYears = Object.keys(groupedByYear)
				.map((year) => parseInt(year))
				.sort((a, b) => b - a);

			isLoading = false;
		} catch (error) {
			console.error('Error loading activity posts:', error);
			isLoading = false;
		}
	});

	function formatDate(dateString: string): string {
		const date = new Date(dateString);
		return date.toLocaleDateString('id-ID', {
			year: 'numeric',
			month: 'long',
			day: 'numeric'
		});
	}

	function getFilteredPosts(): ActivityPost[] {
		if (selectedYear === 'all') {
			return activityPosts.sort((a, b) => new Date(b.date).getTime() - new Date(a.date).getTime());
		}
		return groupedByYear[selectedYear as number] || [];
	}
</script>

<svelte:head>
	<title>Album Kegiatan - Kampoeng Ku</title>
	<meta name="description" content="Album foto kegiatan dan aktivitas warga RT 8" />
</svelte:head>

<div class="min-h-screen bg-gray-50">
	<div class="container mx-auto px-4 py-8">
		<!-- Header Section -->
		<div class="mb-8 text-center">
			<h1 class="mb-4 text-4xl font-bold text-gray-900">Album Kegiatan RT 8</h1>
			<p class="mb-6 text-lg text-gray-600">Dokumentasi kegiatan dan aktivitas warga RT 8</p>

			<!-- Instagram Attribution -->
			<a href="http://instagram.com/kartar.rt8" target="_blank" rel="noopener noreferrer">
				<div class="mb-6 inline-flex items-center rounded-lg bg-white px-4 py-2 shadow-sm">
					<svg class="mr-2 h-5 w-5 text-pink-500" fill="currentColor" viewBox="0 0 24 24">
						<path
							d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"
						/>
					</svg>
					<span class="text-gray-700">@kartar.rt8</span>
				</div>
			</a>
			<!-- Tiktok Attribution -->
			<a href="http://tiktok.com/@kartar_rt08" target="_blank" rel="noopener noreferrer">
				<div class="mb-6 inline-flex items-center rounded-lg bg-white px-4 py-2 shadow-sm">
					<svg class="mr-2 h-5 w-5 text-black" fill="currentColor" viewBox="0 0 24 24">
						<path
							d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-.04-.1z"
						/>
					</svg>
					<span class="text-gray-700">@kartar_rt08</span>
				</div>
			</a>
		</div>

		{#if isLoading}
			<div class="flex items-center justify-center py-20">
				<div class="flex items-center space-x-3">
					<div class="h-8 w-8 animate-spin rounded-full border-b-2 border-blue-600"></div>
					<span class="text-gray-600">Memuat album kegiatan...</span>
				</div>
			</div>
		{:else}
			<!-- Filter Section -->
			<div class="mb-8 flex justify-center">
				<div class="inline-flex rounded-lg bg-white p-1 shadow-sm">
					<button
						class="rounded-md px-4 py-2 text-sm font-medium transition-colors duration-200 {selectedYear ===
						'all'
							? 'bg-blue-600 text-white'
							: 'text-gray-700 hover:text-blue-600'}"
						onclick={() => (selectedYear = 'all')}
					>
						Semua Tahun
					</button>
					{#each availableYears as year}
						<button
							class="rounded-md px-4 py-2 text-sm font-medium transition-colors duration-200 {selectedYear ===
							year
								? 'bg-blue-600 text-white'
								: 'text-gray-700 hover:text-blue-600'}"
							onclick={() => (selectedYear = year)}
						>
							{year}
						</button>
					{/each}
				</div>
			</div>

			<!-- Posts Grid -->
			{#if getFilteredPosts().length > 0}
				<div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
					{#each getFilteredPosts() as post (post.id)}
						<div
							class="group overflow-hidden rounded-xl bg-white shadow-sm transition-shadow duration-300 hover:shadow-lg"
						>
							<!-- Image -->
							<div class="relative overflow-hidden">
								<img
									src={post.imageUrl}
									alt={post.caption}
									class="h-64 w-full object-cover transition-transform duration-300 group-hover:scale-105"
									loading="lazy"
								/>

								<!-- Year Badge -->
								<div class="absolute top-3 left-3">
									<span class="bg-opacity-70 rounded-full bg-black px-2 py-1 text-xs text-white">
										{post.year}
									</span>
								</div>

								<!-- Likes Badge -->
								<!-- {#if post.likes}
                                    <div class="absolute top-3 right-3">
                                        <div class="bg-black bg-opacity-70 text-white text-xs px-2 py-1 rounded-full flex items-center">
                                            <svg class="w-3 h-3 mr-1 text-red-400" fill="currentColor" viewBox="0 0 24 24">
                                                <path d="M12,21.35L10.55,20.03C5.4,15.36 2,12.27 2,8.5 2,5.41 4.42,3 7.5,3C9.24,3 10.91,3.81 12,5.08C13.09,3.81 14.76,3 16.5,3C19.58,3 22,5.41 22,8.5C22,12.27 18.6,15.36 13.45,20.03L12,21.35Z" />
                                            </svg>
                                            {post.likes}
                                        </div>
                                    </div>
                                {/if} -->
							</div>

							<!-- Content -->
							<div class="p-4">
								<p class="mb-2 line-clamp-2 text-sm font-medium text-gray-800">{post.caption}</p>

								<div class="flex items-center justify-between text-xs text-gray-500">
									<span class="flex items-center">
										<svg class="mr-1 h-3 w-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
											></path>
										</svg>
										{formatDate(post.date)}
									</span>
								</div>

								{#if post.location}
									<div class="mt-2 flex items-center text-xs text-gray-500">
										<svg class="mr-1 h-3 w-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
											></path>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
											></path>
										</svg>
										{post.location}
									</div>
								{/if}
							</div>
						</div>
					{/each}
				</div>
			{:else}
				<div class="py-20 text-center">
					<div
						class="mb-4 inline-flex h-16 w-16 items-center justify-center rounded-full bg-gray-100"
					>
						<svg
							class="h-8 w-8 text-gray-400"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
							></path>
						</svg>
					</div>
					<h3 class="mb-2 text-lg font-medium text-gray-900">Belum Ada Kegiatan</h3>
					<p class="text-gray-500">Foto kegiatan akan ditampilkan di sini.</p>
				</div>
			{/if}

			<!-- Statistics Section -->
			<!-- {#if activityPosts.length > 0}
                <div class="mt-16 bg-white rounded-xl shadow-sm p-8">
                    <h2 class="text-2xl font-bold text-gray-900 mb-6 text-center">Statistik Kegiatan</h2>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="text-center p-4 bg-blue-50 rounded-lg">
                            <div class="text-3xl font-bold text-blue-600 mb-2">{activityPosts.length}</div>
                            <div class="text-gray-600">Total Kegiatan</div>
                        </div>
                        <div class="text-center p-4 bg-green-50 rounded-lg">
                            <div class="text-3xl font-bold text-green-600 mb-2">{availableYears.length}</div>
                            <div class="text-gray-600">Tahun Aktif</div>
                        </div>
                        <div class="text-center p-4 bg-purple-50 rounded-lg">
                            <div class="text-3xl font-bold text-purple-600 mb-2">
                                {activityPosts.reduce((sum, post) => sum + (post.likes || 0), 0)}
                            </div>
                            <div class="text-gray-600">Total Likes</div>
                        </div>
                    </div>
                </div>
            {/if} -->
		{/if}
	</div>
</div>

<style>
	.line-clamp-2 {
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		overflow: hidden;
	}
</style>
