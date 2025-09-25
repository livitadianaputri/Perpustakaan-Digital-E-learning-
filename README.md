<!DOCTYPE html>
<html lang="id" class="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Perpustakaan Digital</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        /* Custom scrollbar for better aesthetics */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
        }
        ::-webkit-scrollbar-thumb {
            background: #888;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #555;
        }
        .dark ::-webkit-scrollbar-track {
            background: #2d3748;
        }
        .dark ::-webkit-scrollbar-thumb {
            background: #718096;
        }
        .dark ::-webkit-scrollbar-thumb:hover {
            background: #a0aec0;
        }
        /* Style for modal transition */
        #book-modal, #reading-modal {
            transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out;
        }
        /* Basic prose styling for reading view */
        #reading-text h1 {
            font-size: 2.25rem; /* 36px */
            line-height: 2.5rem; /* 40px */
            font-weight: 700;
            margin-bottom: 1rem;
        }
        #reading-text h2 {
            font-size: 1.5rem; /* 24px */
            line-height: 2rem; /* 32px */
            font-weight: 600;
            margin-bottom: 0.75rem;
            color: #4b5563; /* gray-600 */
        }
        #reading-text p {
            margin-bottom: 1rem;
            line-height: 1.75;
        }
        .dark #reading-text h2 {
            color: #9ca3af; /* gray-400 */
        }
    </style>
    <script>
        // Set theme on initial load
        if (localStorage.getItem('theme') === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }
    </script>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-200 transition-colors duration-300">

    <div id="app" class="container mx-auto p-4 sm:p-6 lg:p-8">
        <!-- Header Section -->
        <header class="flex flex-col sm:flex-row justify-between items-center mb-8">
            <h1 class="text-3xl sm:text-4xl font-bold text-indigo-600 dark:text-indigo-400 mb-4 sm:mb-0">
                Perpustakaan Digital
            </h1>
            <div class="flex items-center space-x-4 w-full sm:w-auto">
                <div class="relative flex-grow">
                    <input type="text" id="search-input" placeholder="Cari judul atau penulis..." class="w-full pl-10 pr-4 py-2 border rounded-full bg-white dark:bg-gray-800 border-gray-300 dark:border-gray-600 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <svg class="w-5 h-5 absolute left-3 top-1/2 -translate-y-1/2 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                    </svg>
                </div>
                <button id="theme-toggle" class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 dark:focus:ring-offset-gray-900">
                    <svg id="theme-icon-light" class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path></svg>
                    <svg id="theme-icon-dark" class="w-6 h-6 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path></svg>
                </button>
            </div>
        </header>

        <!-- Book Grid Section -->
        <main id="book-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-6">
            <!-- Book cards will be injected here by JavaScript -->
        </main>
        
        <div id="no-results" class="hidden text-center py-16">
            <h2 class="text-2xl font-semibold">Buku tidak ditemukan</h2>
            <p class="text-gray-500 dark:text-gray-400 mt-2">Coba gunakan kata kunci lain.</p>
        </div>
    </div>

    <!-- Book Detail Modal -->
    <div id="book-modal" class="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 z-50 invisible opacity-0">
        <div id="modal-content" class="bg-white dark:bg-gray-800 rounded-lg shadow-2xl w-full max-w-4xl max-h-[90vh] flex flex-col md:flex-row transform scale-95 transition-transform duration-300">
            <!-- Modal content will be injected here by JavaScript -->
        </div>
    </div>

    <!-- Reading View Modal -->
    <div id="reading-modal" class="fixed inset-0 bg-white dark:bg-gray-900 z-[60] invisible opacity-0">
        <div id="reading-content" class="h-full flex flex-col">
            <!-- Header -->
            <header class="flex items-center justify-between p-4 border-b border-gray-200 dark:border-gray-700 flex-shrink-0">
                <h3 id="reading-title" class="text-xl font-bold text-gray-800 dark:text-gray-200 truncate pr-4"></h3>
                <button id="close-reading-view" class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700">
                     <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                </button>
            </header>
            <!-- Content -->
            <div id="reading-text" class="p-6 md:p-8 lg:p-12 overflow-y-auto flex-grow">
                <!-- Book text will be injected here -->
            </div>
        </div>
    </div>


    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- DATA BUKU (Contoh) ---
            const books = [
                {
                    id: 1,
                    title: "Laskar Pelangi",
                    author: "Andrea Hirata",
                    year: 2005,
                    cover_url: "https://placehold.co/400x600/6366f1/white?text=Laskar+Pelangi",
                    synopsis: "Kisah inspiratif tentang persahabatan dan perjuangan 10 anak di Belitung untuk mendapatkan pendidikan yang layak. Mereka bersekolah di SD Muhammadiyah Gantong yang serba terbatas namun penuh dengan semangat dan mimpi.",
                    rating: 5,
                    loan_status: "Tersedia"
                },
                {
                    id: 2,
                    title: "Bumi Manusia",
                    author: "Pramoedya Ananta Toer",
                    year: 1980,
                    cover_url: "https://placehold.co/400x600/ec4899/white?text=Bumi+Manusia",
                    synopsis: "Mengisahkan perjalanan Minke, seorang priyayi Jawa yang cerdas di masa kolonial Belanda. Novel ini menyoroti perjuangan melawan diskriminasi, pencarian identitas, dan cinta di tengah gejolak politik awal abad ke-20.",
                    rating: 5,
                    loan_status: "Dipinjam"
                },
                {
                    id: 3,
                    title: "Perahu Kertas",
                    author: "Dee Lestari",
                    year: 2009,
                    cover_url: "https://placehold.co/400x600/22c55e/white?text=Perahu+Kertas",
                    synopsis: "Kisah cinta yang unik antara Kugy, seorang gadis eksentrik yang gemar membuat perahu kertas, dan Keenan, seorang pelukis berbakat. Takdir mempertemukan dan memisahkan mereka berulang kali dalam sebuah alur yang puitis.",
                    rating: 4,
                    loan_status: "Tersedia"
                },
                {
                    id: 4,
                    title: "Negeri 5 Menara",
                    author: "Ahmad Fuadi",
                    year: 2009,
                    cover_url: "https://placehold.co/400x600/f97316/white?text=Negeri+5+Menara",
                    synopsis: "Alif Fikri, seorang anak dari Maninjau, harus merelakan mimpinya masuk SMA di Bukittinggi dan mengikuti keinginan ibunya untuk bersekolah di Pondok Madani. Dengan mantra 'Man Jadda Wajada', ia dan teman-temannya berjuang meraih mimpi mereka.",
                    rating: 5,
                    loan_status: "Tersedia"
                },
                {
                    id: 5,
                    title: "Cantik Itu Luka",
                    author: "Eka Kurniawan",
                    year: 2002,
                    cover_url: "https://placehold.co/400x600/ef4444/white?text=Cantik+Itu+Luka",
                    synopsis: "Sebuah novel epik dengan gaya realisme magis yang menceritakan kisah tragis Dewi Ayu dan keturunannya. Latar belakang sejarah Indonesia dari masa kolonial hingga Orde Baru menjadi panggung bagi kisah-kisah hantu, kekerasan, dan cinta.",
                    rating: 4,
                    loan_status: "Dipinjam"
                },
                {
                    id: 6,
                    title: "Filosofi Kopi",
                    author: "Dee Lestari",
                    year: 2006,
                    cover_url: "https://placehold.co/400x600/8b5cf6/white?text=Filosofi+Kopi",
                    synopsis: "Kumpulan cerita dan prosa yang mengajak pembaca merenungkan makna hidup melalui secangkir kopi. Cerita utamanya berpusat pada Ben dan Jody, dua sahabat yang mendirikan kedai kopi 'Filosofi Kopi' dan menemukan makna di setiap racikan kopi mereka.",
                    rating: 4,
                    loan_status: "Tersedia"
                },
                {
                    id: 7,
                    title: "Sapiens: Riwayat Singkat Umat Manusia",
                    author: "Yuval Noah Harari",
                    year: 2011,
                    cover_url: "https://placehold.co/400x600/06b6d4/white?text=Sapiens",
                    synopsis: "Sebuah penjelajahan mendalam tentang sejarah umat manusia, dari zaman batu hingga era modern. Harari menjelaskan bagaimana Homo Sapiens berhasil mendominasi planet ini melalui revolusi kognitif, agrikultur, dan sains.",
                    rating: 5,
                    loan_status: "Tersedia"
                },
                 {
                    id: 8,
                    title: "Gadis Kretek",
                    author: "Ratih Kumala",
                    year: 2012,
                    cover_url: "https://placehold.co/400x600/78716c/white?text=Gadis+Kretek",
                    synopsis: "Pencarian seorang perempuan misterius bernama Jeng Yah membawa tiga bersaudara menelusuri jejak bisnis kretek ayahnya di masa lalu. Novel ini adalah perpaduan kisah cinta, sejarah industri kretek, dan rahasia keluarga yang kelam.",
                    rating: 5,
                    loan_status: "Dipinjam"
                }
            ];

            // --- DOM Elements ---
            const bookGrid = document.getElementById('book-grid');
            const searchInput = document.getElementById('search-input');
            const themeToggle = document.getElementById('theme-toggle');
            const themeIconLight = document.getElementById('theme-icon-light');
            const themeIconDark = document.getElementById('theme-icon-dark');
            const bookModal = document.getElementById('book-modal');
            const modalContent = document.getElementById('modal-content');
            const noResults = document.getElementById('no-results');

            // --- Gemini API Function ---
            const callGeminiApi = async (book) => {
                const aiContentDiv = document.getElementById('ai-insights-content');
                const aiButton = document.getElementById('ai-insights-button');
                
                aiContentDiv.innerHTML = `
                    <div class="flex items-center justify-center gap-2 text-gray-500 dark:text-gray-400">
                        <div class="animate-spin rounded-full h-5 w-5 border-b-2 border-indigo-500"></div>
                        <span>Menganalisa buku...</span>
                    </div>`;
                aiButton.disabled = true;
                aiButton.classList.add('cursor-not-allowed', 'opacity-50');

                const apiKey = ""; // API key is handled by the environment, leave empty.
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                const systemPrompt = "You are a helpful literary assistant. Analyze the provided book details and generate a concise, bullet-point summary and a short analysis. Respond in Bahasa Indonesia. Format your response using Markdown.";
                const userQuery = `
                    Analisa buku berikut:
                    Judul: ${book.title}
                    Penulis: ${book.author}
                    Sinopsis: ${book.synopsis}

                    Berikan jawaban dalam format Markdown berikut:
                    ### Ringkasan Poin
                    - Poin 1
                    - Poin 2
                    - Poin 3

                    ### Analisa Singkat
                    Analisa singkat mengenai tema utama, target pembaca, atau keunikan dari buku ini.
                `;

                const payload = {
                    contents: [{ parts: [{ text: userQuery }] }],
                    systemInstruction: { parts: [{ text: systemPrompt }] },
                };

                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    if (!response.ok) {
                        throw new Error(`API request failed with status ${response.status}`);
                    }

                    const result = await response.json();
                    const candidate = result.candidates?.[0];

                    if (candidate && candidate.content?.parts?.[0]?.text) {
                        let text = candidate.content.parts[0].text;
                        // Simple Markdown to HTML conversion
                        text = text.replace(/### (.*)/g, '<h5 class="font-semibold text-md mt-4 mb-2 text-gray-800 dark:text-gray-200">$1</h5>');
                        text = text.replace(/\* (.*)/g, '<li class="ml-5 list-disc">$1</li>');
                        text = text.replace(/\- (.*)/g, '<li class="ml-5 list-disc">$1</li>');
                        text = text.replace(/(\r\n|\n|\r)/gm, "<br>");
                        text = text.replace(/<br><li/g, '<li');
                        text = text.replace(/<\/li><br>/g, '</li>');

                        aiContentDiv.innerHTML = `<ul>${text}</ul>`;
                    } else {
                        throw new Error("No content received from API.");
                    }
                } catch (error) {
                    console.error("Gemini API Error:", error);
                    aiContentDiv.innerHTML = `<p class="text-red-500 text-center">Maaf, terjadi kesalahan saat menganalisa buku. Silakan coba lagi nanti.</p>`;
                }
            };

            // --- RENDER FUNCTIONS ---
            
            const renderStars = (rating) => {
                let stars = '';
                for (let i = 1; i <= 5; i++) {
                    const color = i <= rating ? 'text-yellow-400' : 'text-gray-300 dark:text-gray-500';
                    stars += `<svg class="w-5 h-5 ${color}" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"></path></svg>`;
                }
                return `<div class="flex items-center">${stars}</div>`;
            };

            const renderBooks = (booksToRender) => {
                bookGrid.innerHTML = '';
                if(booksToRender.length === 0){
                    noResults.classList.remove('hidden');
                    return;
                }
                noResults.classList.add('hidden');

                booksToRender.forEach(book => {
                    const statusColor = book.loan_status === 'Tersedia' ? 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300' : 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300';
                    const bookCard = `
                        <div class="book-card cursor-pointer group bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden transform hover:-translate-y-2 transition-all duration-300" data-id="${book.id}">
                            <img src="${book.cover_url}" alt="Cover ${book.title}" class="w-full h-64 object-cover">
                            <div class="p-4">
                                <h3 class="font-bold text-md truncate group-hover:text-indigo-600 dark:group-hover:text-indigo-400">${book.title}</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">${book.author}</p>
                                <div class="flex justify-between items-center mt-3">
                                    ${renderStars(book.rating)}
                                    <span class="text-xs font-semibold px-2 py-1 rounded-full ${statusColor}">${book.loan_status}</span>
                                </div>
                            </div>
                        </div>
                    `;
                    bookGrid.innerHTML += bookCard;
                });
            };

            const showBookDetail = (bookId) => {
                const book = books.find(b => b.id === bookId);
                if (!book) return;

                const statusColor = book.loan_status === 'Tersedia' ? 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300' : 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300';
                const buttonClass = book.loan_status === 'Tersedia' 
                    ? 'bg-indigo-600 hover:bg-indigo-700 text-white' 
                    : 'bg-gray-400 cursor-not-allowed text-gray-800 dark:bg-gray-600 dark:text-gray-400';
                const buttonText = book.loan_status === 'Tersedia' ? 'Download E-Book' : 'Tidak Tersedia';

                modalContent.innerHTML = `
                    <div class="w-full md:w-1/3 flex-shrink-0">
                        <img src="${book.cover_url}" alt="Cover ${book.title}" class="w-full h-full object-cover rounded-t-lg md:rounded-l-lg md:rounded-t-none">
                    </div>
                    <div class="p-6 flex flex-col overflow-y-auto">
                        <div class="flex-grow">
                            <div class="flex justify-between items-start">
                                <h2 class="text-3xl font-bold mb-2 text-indigo-600 dark:text-indigo-400">${book.title}</h2>
                                <button id="close-modal" class="p-1 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 -mt-2 -mr-2">
                                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                                </button>
                            </div>
                            <p class="text-lg text-gray-700 dark:text-gray-300 mb-2">${book.author} - <span class="text-gray-500 dark:text-gray-400">${book.year}</span></p>
                            <div class="flex items-center mb-4">
                                ${renderStars(book.rating)}
                                <span class="ml-2 text-gray-600 dark:text-gray-400">(${book.rating}.0)</span>
                            </div>
                            <h4 class="font-semibold text-lg mt-4 mb-2 border-b border-gray-200 dark:border-gray-700 pb-1">Sinopsis</h4>
                            <p class="text-gray-600 dark:text-gray-400 text-base leading-relaxed">${book.synopsis}</p>
                            
                            <div class="mt-6 pt-4 border-t border-gray-200 dark:border-gray-700">
                                <button id="ai-insights-button" class="flex items-center justify-center gap-2 w-full sm:w-auto px-4 py-2 font-semibold rounded-lg transition-colors duration-200 bg-purple-600 hover:bg-purple-700 text-white">
                                    ✨ Ringkas & Analisa dengan AI
                                </button>
                                <div id="ai-insights-content" class="mt-4 text-gray-600 dark:text-gray-400 text-base leading-relaxed">
                                    <!-- AI content will appear here -->
                                </div>
                            </div>
                        </div>

                        <div class="mt-6 pt-6 border-t border-gray-200 dark:border-gray-700 flex-shrink-0 flex flex-col sm:flex-row items-center justify-between gap-4">
                            <div class="flex items-center">
                                <span class="font-semibold mr-2">Status:</span>
                                <span class="text-sm font-bold px-3 py-1 rounded-full ${statusColor}">${book.loan_status}</span>
                            </div>
                            <div class="flex gap-4">
                                 <button id="read-now-button" class="px-5 py-2.5 font-semibold rounded-lg transition-colors duration-200 border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-600 hover:text-white dark:border-indigo-400 dark:text-indigo-400 dark:hover:bg-indigo-400 dark:hover:text-gray-900">
                                    Baca Sekarang
                                </button>
                                <button class="px-5 py-2.5 font-semibold rounded-lg transition-colors duration-200 ${buttonClass}" ${book.loan_status !== 'Tersedia' ? 'disabled' : ''}>
                                    ${buttonText}
                                </button>
                            </div>
                        </div>
                    </div>
                `;

                bookModal.classList.remove('invisible', 'opacity-0');
                setTimeout(() => modalContent.classList.remove('scale-95'), 10); // Start transition

                document.getElementById('close-modal').addEventListener('click', closeModal);
                document.getElementById('ai-insights-button').addEventListener('click', () => callGeminiApi(book));
                document.getElementById('read-now-button').addEventListener('click', () => showReadingView(book));
            };
            
            const closeModal = () => {
                modalContent.classList.add('scale-95');
                bookModal.classList.add('opacity-0');
                setTimeout(() => {
                    bookModal.classList.add('invisible');
                }, 300); // Wait for transition to finish
            };

            const showReadingView = (book) => {
                const readingModal = document.getElementById('reading-modal');
                const readingTitle = document.getElementById('reading-title');
                const readingText = document.getElementById('reading-text');
                
                readingTitle.textContent = book.title;
                // Simulate book content by repeating the synopsis
                let bookContent = `<h1 class="text-3xl font-bold mb-4">${book.title}</h1><h2 class="text-xl text-gray-600 dark:text-gray-400 mb-8">${book.author}</h2>`;
                for (let i = 0; i < 15; i++) {
                    bookContent += `<p class="mb-4">${book.synopsis} (Paragraf simulasi ke-${i + 1}) Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>`;
                }
                readingText.innerHTML = bookContent;
                readingText.scrollTop = 0; // Scroll to top

                readingModal.classList.remove('invisible', 'opacity-0');
            };

            const closeReadingView = () => {
                const readingModal = document.getElementById('reading-modal');
                readingModal.classList.add('opacity-0');
                setTimeout(() => {
                    readingModal.classList.add('invisible');
                }, 300);
            };

            // --- EVENT HANDLERS ---
            
            const handleSearch = () => {
                const query = searchInput.value.toLowerCase();
                const filteredBooks = books.filter(book => 
                    book.title.toLowerCase().includes(query) || 
                    book.author.toLowerCase().includes(query)
                );
                renderBooks(filteredBooks);
            };

            const toggleTheme = () => {
                const isDark = document.documentElement.classList.toggle('dark');
                localStorage.setItem('theme', isDark ? 'dark' : 'light');
                updateThemeIcons(isDark);
            };

            const updateThemeIcons = (isDark) => {
                if (isDark) {
                    themeIconLight.classList.add('hidden');
                    themeIconDark.classList.remove('hidden');
                } else {
                    themeIconLight.classList.remove('hidden');
                    themeIconDark.classList.add('hidden');
                }
            };
            
            // --- INITIALIZATION ---
            
            // Initial render
            renderBooks(books);
            updateThemeIcons(document.documentElement.classList.contains('dark'));

            // Event Listeners
            searchInput.addEventListener('input', handleSearch);
            themeToggle.addEventListener('click', toggleTheme);
            document.getElementById('close-reading-view').addEventListener('click', closeReadingView);
            
            bookGrid.addEventListener('click', (e) => {
                const card = e.target.closest('.book-card');
                if (card) {
                    const bookId = parseInt(card.dataset.id, 10);
                    showBookDetail(bookId);
                }
            });

            bookModal.addEventListener('click', (e) => {
                if (e.target === bookModal) {
                    closeModal();
                }
            });
            
            document.addEventListener('keydown', (e) => {
                if (e.key === 'Escape') {
                    if (!document.getElementById('reading-modal').classList.contains('invisible')) {
                        closeReadingView();
                    } else if (!bookModal.classList.contains('invisible')) {
                        closeModal();
                    }
                }
            });
        });
    </script>
</body>
</html>
