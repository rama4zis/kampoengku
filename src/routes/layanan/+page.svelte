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
    <meta name="description" content="Daftar layanan administrasi dan pelayanan publik yang tersedia" />
</svelte:head>

<div class="max-w-4xl mx-auto p-6">
    <div class="mb-8 text-center">
        <h1 class="text-3xl font-bold text-gray-200 mb-4">Layanan Administrasi</h1>
        <p class="text-lg text-gray-600">Daftar layanan dan persyaratan yang diperlukan</p>
    </div>
    
    <div class="space-y-4">
        {#each services as service, index}
            <div class="border border-gray-200 rounded-lg shadow-sm">
                <button 
                    class="w-full px-6 py-4 text-left bg-gray-50 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-blue-500 rounded-t-lg transition-colors duration-200"
                    onclick={() => toggleItem(index)}
                >
                    <div class="flex justify-between items-center">
                        <span class="font-semibold text-gray-900 text-lg">{service.title}</span>
                        <svg 
                            class="w-5 h-5 text-gray-500 transform transition-transform duration-200 {openItems[index] ? 'rotate-180' : ''}" 
                            fill="none" 
                            stroke="currentColor" 
                            viewBox="0 0 24 24"
                        >
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
                        </svg>
                    </div>
                </button>
                {#if openItems[index]}
                    <div class="px-6 py-4 bg-white border-t border-gray-200 overflow-hidden" 
                         transition:slide={{ duration: 300, easing: quintOut }}>
                        <h3 class="font-semibold text-gray-800 mb-3">Persyaratan:</h3>
                        <ul class="space-y-2">
                            {#each service.requirements as requirement}
                                <li class="flex items-start">
                                    <span class="inline-block w-2 h-2 bg-blue-500 rounded-full mt-2 mr-3 flex-shrink-0"></span>
                                    <span class="text-gray-700">{requirement}</span>
                                </li>
                            {/each}
                        </ul>
                    </div>
                {/if}
            </div>
        {/each}
        
        {#if services.length === 0}
            <div class="text-center py-12">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-gray-100 rounded-full mb-4">
                    <svg class="w-8 h-8 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path>
                    </svg>
                </div>
                <p class="text-gray-500">Memuat data layanan...</p>
            </div>
        {/if}
    </div>
</div>