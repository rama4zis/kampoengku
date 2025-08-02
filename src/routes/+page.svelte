<script>
	import { onMount } from 'svelte';

	let displayText = '';
	let fullText = 'Welcome to Bratang Gede 3i - Surabaya';
	let currentIndex = 0;
	let isTyping = true;

	onMount(() => {
		const typeWriter = () => {
			if (isTyping) {
				if (currentIndex < fullText.length) {
					displayText += fullText.charAt(currentIndex);
					currentIndex++;
				} else {
					isTyping = false;
					setTimeout(() => {
						isTyping = false;
						currentIndex = fullText.length;
						const deleteInterval = setInterval(() => {
							if (currentIndex > 0) {
								displayText = displayText.slice(0, -1);
								currentIndex--;
							} else {
								clearInterval(deleteInterval);
								isTyping = true;
								currentIndex = 0;
							}
						}, 50); // Delete speed (faster than typing)
					}, 2000); // Wait 2 seconds before deleting
				}
			}
		};

		const interval = setInterval(typeWriter, 100); // Type speed
		return () => clearInterval(interval);
	});
</script>

<div class="container" id="home">
	<h1 class="typing-text">{displayText}<span class="cursor">|</span></h1>
</div>
<div class="flex flex-wrap justify-center gap-4 rounded-lg bg-gray-900 p-4 text-white shadow-lg">
	<!-- Profile -->
	<section
		class="flex h-32 w-32 flex-col items-center gap-2 rounded-lg bg-gray-800 p-4 text-white shadow-lg"
	>
		<!-- Icon -->
		<div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-600">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="size-6"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M3.75 21h16.5M4.5 3h15M5.25 3v18m13.5-18v18M9 6.75h1.5m-1.5 3h1.5m-1.5 3h1.5m3-6H15m-1.5 3H15m-1.5 3H15M9 21v-3.375c0-.621.504-1.125 1.125-1.125h3.75c.621 0 1.125.504 1.125 1.125V21"
				/>
			</svg>
		</div>
		<a href="/profile"><h2 class="text-lg font-semibold">Profile</h2></a>
	</section>

	<!-- Activity -->
	<section
		class="flex h-32 w-32 flex-col items-center gap-2 rounded-lg bg-gray-800 p-4 text-white shadow-lg"
	>
		<!-- Icon -->
		<div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-600">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="size-6"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="m3.75 13.5 10.5-11.25L12 10.5h8.25L9.75 21.75 12 13.5H3.75Z"
				/>
			</svg>
		</div>
		<a href="/activity"><h2 class="text-lg font-semibold">Activity</h2></a>
	</section>

	<!-- Inventory -->
	<section
		class="flex h-32 w-32 flex-col items-center gap-2 rounded-lg bg-gray-800 p-4 text-white shadow-lg"
	>
		<!-- Icon -->
		<div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-600">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="size-6"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5M10 11.25h4M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z"
				/>
			</svg>
		</div>
		<a href="/inventory"><h2 class="text-lg font-semibold">Inventory</h2></a>
	</section>

	<!-- Award -->
	<section
		class="flex h-32 w-32 flex-col items-center gap-2 rounded-lg bg-gray-800 p-4 text-white shadow-lg"
	>
		<!-- Icon -->
		<div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-600">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="size-6"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M9.813 15.904 9 18.75l-.813-2.846a4.5 4.5 0 0 0-3.09-3.09L2.25 12l2.846-.813a4.5 4.5 0 0 0 3.09-3.09L9 5.25l.813 2.846a4.5 4.5 0 0 0 3.09 3.09L15.75 12l-2.846.813a4.5 4.5 0 0 0-3.09 3.09ZM18.259 8.715 18 9.75l-.259-1.035a3.375 3.375 0 0 0-2.455-2.456L14.25 6l1.036-.259a3.375 3.375 0 0 0 2.455-2.456L18 2.25l.259 1.035a3.375 3.375 0 0 0 2.456 2.456L21.75 6l-1.035.259a3.375 3.375 0 0 0-2.456 2.456ZM16.894 20.567 16.5 21.75l-.394-1.183a2.25 2.25 0 0 0-1.423-1.423L13.5 18.75l1.183-.394a2.25 2.25 0 0 0 1.423-1.423l.394-1.183.394 1.183a2.25 2.25 0 0 0 1.423 1.423l1.183.394-1.183.394a2.25 2.25 0 0 0-1.423 1.423Z"
				/>
			</svg>
		</div>
		<a href="/award"><h2 class="text-lg font-semibold">Award</h2></a>
	</section>

	<!-- Management -->
	<section
		class="flex h-32 w-32 flex-col items-center gap-2 rounded-lg bg-gray-800 p-4 text-white shadow-lg"
	>
		<!-- Icon -->
		<div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-600">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="size-6"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z"
				/>
			</svg>
		</div>
		<a href="/management"><h2 class="text-lg font-semibold">Management</h2></a>
	</section>

	<!-- Finance -->
	<section
		class="flex h-32 w-32 flex-col items-center gap-2 rounded-lg bg-gray-800 p-4 text-white shadow-lg"
	>
		<!-- Icon -->
		<div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-600">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="size-6"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M12 6v12m-3-2.818.879.659c1.171.879 3.07.879 4.242 0 1.172-.879 1.172-2.303 0-3.182C13.536 12.219 12.768 12 12 12c-.725 0-1.45-.22-2.003-.659-1.106-.879-1.106-2.303 0-3.182s2.9-.879 4.006 0l.415.33M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
				/>
			</svg>
		</div>
		<a href="/finance"><h2 class="text-lg font-semibold">Finance</h2></a>
	</section>

	<!-- Service -->
	<section
		class="flex h-32 w-32 flex-col items-center gap-2 rounded-lg bg-gray-800 p-4 text-white shadow-lg"
	>
		<!-- Icon -->
		<div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-600">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="size-6"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M2.25 12.76c0 1.6 1.123 2.994 2.707 3.227 1.087.16 2.185.283 3.293.369V21l4.076-4.076a1.526 1.526 0 0 1 1.037-.443 48.282 48.282 0 0 0 5.68-.494c1.584-.233 2.707-1.626 2.707-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0 0 12 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018Z"
				/>
			</svg>
		</div>
		<a href="/service"><h2 class="text-lg font-semibold">Service</h2></a>
	</section>
</div>

<style>
	#home {
		display: flex;
		justify-content: center;
		align-items: center;
		min-height: 50vh;
	}

	.typing-text {
		font-size: 3rem;
		font-weight: bold;
		text-align: center;
		color: #ebebeb;
	}

	.cursor {
		animation: blink 1s infinite;
	}

	@keyframes blink {
		0%,
		50% {
			opacity: 1;
		}
		51%,
		100% {
			opacity: 0;
		}
	}

	@media (max-width: 768px) {
		.typing-text {
			font-size: 2rem;
		}
	}
</style>
