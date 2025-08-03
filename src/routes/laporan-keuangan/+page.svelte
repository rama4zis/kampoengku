<script lang="ts">
    import { onMount } from 'svelte';
    
    interface LaporanKeuangan {
        name: string;
        url: string;
        year: string;
        size?: string;
    }
    
    let laporanKeuangan: LaporanKeuangan[] = [];
    let isLoading = true;
    
    onMount(async () => {
        try {
            // Simulasi data laporan keuangan (karena tidak bisa listing files dari static folder secara dinamis)
            // Dalam implementasi nyata, ini bisa menggunakan API endpoint
            laporanKeuangan = [
                {
                    name: "Laporan Keuangan 2025",
                    url: "/data/laporan-keuangan/laporan-keuangan-2025.pdf",
                    year: "2025",
                    // size: "2.5 MB"
                },
                {
                    name: "Laporan Keuangan 2024",
                    url: "/data/laporan-keuangan/laporan-keuangan-2024.pdf",
                    year: "2024",
                    // size: "2.5 MB"
                },
                {
                    name: "Laporan Keuangan 2023", 
                    url: "/data/laporan-keuangan/laporan-keuangan-2023.pdf",
                    year: "2023",
                    // size: "2.1 MB"
                },
                {
                    name: "Laporan Keuangan 2022",
                    url: "/data/laporan-keuangan/laporan-keuangan-2022.pdf", 
                    year: "2022",
                    // size: "1.8 MB"
                }
            ];
            isLoading = false;
        } catch (error) {
            console.error('Error loading laporan keuangan:', error);
            isLoading = false;
        }
    });
    
    function downloadPDF(url: string, filename: string) {
        const link = document.createElement('a');
        link.href = url;
        link.download = filename;
        link.target = '_blank';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }
</script>

<svelte:head>
    <title>Laporan Keuangan</title>
    <meta name="description" content="Laporan Keuangan Tahunan" />
</svelte:head>


<div class="container mx-auto px-4 py-8">
    <div class="mb-8">
        <h1 class="text-3xl font-bold text-gray-200 mb-4">Laporan Keuangan Tahunan</h1>
        <p class="text-lg text-gray-600 mb-6">Laporan keuangan tahunan memberikan gambaran menyeluruh tentang kondisi keuangan organisasi selama satu tahun fiskal.</p>
    </div>

    {#if isLoading}
        <div class="flex justify-center items-center py-12">
            <div class="flex items-center space-x-3">
                <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
                <span class="text-gray-600">Memuat laporan keuangan...</span>
            </div>
        </div>
    {:else if laporanKeuangan.length > 0}
        <div class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
            {#each laporanKeuangan as laporan}
                <div class="bg-white rounded-lg border border-gray-200 shadow-sm hover:shadow-md transition-shadow duration-200">
                    <div class="p-6">
                        <div class="flex items-start justify-between mb-4">
                            <div class="flex-1">
                                <h3 class="text-lg font-semibold text-gray-900 mb-2">{laporan.name}</h3>
                                <div class="flex items-center space-x-4 text-sm text-gray-500">
                                    <span class="flex items-center">
                                        <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                                        </svg>
                                        {laporan.year}
                                    </span>
                                    {#if laporan.size}
                                        <span class="flex items-center">
                                            <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path>
                                            </svg>
                                            {laporan.size}
                                        </span>
                                    {/if}
                                </div>
                            </div>
                            <div class="flex-shrink-0 ml-4">
                                <div class="w-12 h-12 bg-red-100 rounded-lg flex items-center justify-center">
                                    <svg class="w-6 h-6 text-red-600" fill="currentColor" viewBox="0 0 24 24">
                                        <path d="M14,2H6A2,2 0 0,0 4,4V20A2,2 0 0,0 6,22H18A2,2 0 0,0 20,20V8L14,2M18,20H6V4H13V9H18V20Z" />
                                    </svg>
                                </div>
                            </div>
                        </div>
                        
                        <div class="flex space-x-2">
                            <a 
                                href={laporan.url}
                                target="_blank"
                                rel="noopener noreferrer"
                                class="flex-1 inline-flex items-center justify-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition-colors duration-200"
                            >
                                <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path>
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"></path>
                                </svg>
                                Lihat
                            </a>
                            <button 
                                onclick={() => downloadPDF(laporan.url, `${laporan.name}.pdf`)}
                                class="inline-flex items-center justify-center px-4 py-2 border border-transparent text-sm font-medium rounded-md text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition-colors duration-200"
                            >
                                <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path>
                                </svg>
                                Download
                            </button>
                        </div>
                    </div>
                </div>
            {/each}
        </div>
    {:else}
        <div class="text-center py-12">
            <div class="inline-flex items-center justify-center w-16 h-16 bg-gray-100 rounded-full mb-4">
                <svg class="w-8 h-8 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path>
                </svg>
            </div>
            <h3 class="text-lg font-medium text-gray-900 mb-2">Belum Ada Laporan Keuangan</h3>
            <p class="text-gray-500">Laporan keuangan akan ditampilkan di sini ketika tersedia.</p>
        </div>
    {/if}

    <div class="mt-12 bg-blue-50 rounded-lg p-6">
        <h2 class="text-xl font-semibold text-gray-500 mb-3">Informasi Laporan Keuangan</h2>
        <div class="prose prose-blue max-w-none">
            <p class="text-gray-400 mb-4">
                Laporan keuangan ini disusun sesuai dengan standar akuntansi yang berlaku dan telah diaudit oleh auditor independen. 
                Laporan ini mencakup:
            </p>
            <ul class="list-disc list-inside text-gray-400 space-y-1">
                <li>Laporan Posisi Keuangan (Neraca)</li>
                <li>Laporan Laba Rugi dan Penghasilan Komprehensif Lain</li>
                <li>Laporan Perubahan Ekuitas</li>
                <li>Laporan Arus Kas</li>
                <li>Catatan atas Laporan Keuangan</li>
            </ul>
        </div>
    </div>
</div>