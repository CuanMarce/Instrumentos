# Instrumentos
Para identificar los instrumentos presentes en una canción
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Analizador Musical Avanzado</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        .animate-pulse {
            animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        .timeline-instrument {
            transition: all 0.3s ease;
        }
        .timeline-instrument:hover {
            transform: scaleY(1.1);
            box-shadow: 0 0 10px rgba(255,255,255,0.1);
        }
        .waveform {
            background: linear-gradient(90deg, #3b82f6, #8b5cf6);
            height: 80px;
            border-radius: 8px;
            position: relative;
            overflow: hidden;
        }
        .waveform::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, 
                transparent 0%, 
                rgba(255,255,255,0.2) 20%, 
                rgba(255,255,255,0.1) 50%, 
                rgba(255,255,255,0.2) 80%, 
                transparent 100%);
            animation: wave 3s linear infinite;
        }
        @keyframes wave {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }
        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
    </style>
</head>
<body class="bg-gray-900 min-h-screen text-gray-100">
    <div class="container mx-auto px-4 py-8">
        <div class="max-w-6xl mx-auto">
            <!-- Header -->
            <header class="text-center mb-12">
                <h1 class="text-4xl font-bold text-indigo-400 mb-2">Analizador Musical Profesional</h1>
                <p class="text-lg text-gray-400">Detección avanzada de instrumentos y estructura musical</p>
                <div class="mt-4 flex justify-center">
                    <div class="w-16 h-1 bg-indigo-500 rounded-full"></div>
                </div>
            </header>

            <!-- Upload Section -->
            <div class="glass-card rounded-xl shadow-lg p-6 mb-8">
                <div class="flex flex-col md:flex-row items-center gap-6">
                    <div class="flex-1">
                        <h2 class="text-2xl font-semibold text-indigo-300 mb-4">Analiza una canción</h2>
                        <p class="text-gray-400 mb-4">Sube un archivo de audio o proporciona un enlace para analizar la composición musical</p>
                        
                        <div class="mt-6">
                            <label class="flex flex-col items-center justify-center w-full h-32 border-2 border-dashed border-indigo-400 rounded-lg cursor-pointer bg-gray-800 hover:bg-gray-700 transition">
                                <div class="flex flex-col items-center justify-center pt-5 pb-6">
                                    <i class="fas fa-cloud-upload-alt text-3xl text-indigo-400 mb-2"></i>
                                    <p class="text-sm text-gray-400">Arrastra tu archivo aquí o haz clic para seleccionar</p>
                                </div>
                                <input id="file-upload" type="file" class="hidden" accept="audio/*" />
                            </label>
                        </div>
                        
                        <div class="mt-4 flex items-center">
                            <div class="flex-1 h-px bg-gray-700"></div>
                            <span class="px-3 text-gray-500">o</span>
                            <div class="flex-1 h-px bg-gray-700"></div>
                        </div>
                        
                        <div class="mt-4">
                            <div class="flex">
                                <input type="text" placeholder="Pega un enlace de YouTube o Spotify" class="flex-1 px-4 py-2 bg-gray-800 border border-gray-700 rounded-l-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-300">
                                <button class="bg-indigo-600 text-white px-6 py-2 rounded-r-lg hover:bg-indigo-700 transition">Analizar</button>
                            </div>
                        </div>
                    </div>
                    <div class="hidden md:block">
                        <img src="https://images.unsplash.com/photo-1511671782772-c97d3e27f862?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" alt="Music analysis" class="w-64 h-64 object-cover rounded-lg shadow-lg border border-gray-700">
                    </div>
                </div>
            </div>

            <!-- Results Section (Initially hidden) -->
            <div id="results-section" class="hidden">
                <!-- Main Results Grid -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
                    <!-- Column 1: Song Info -->
                    <div class="glass-card rounded-xl p-6">
                        <div class="flex flex-col items-center mb-6">
                            <img id="album-cover" src="https://images.unsplash.com/photo-1470225620780-dba8ba36b745?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=200&q=80" alt="Album cover" class="w-48 h-48 rounded-lg shadow-lg mb-4 border border-gray-700">
                            <h3 id="song-title" class="text-xl font-bold text-center text-indigo-300">Canción de ejemplo</h3>
                            <p id="song-artist" class="text-gray-400 mb-4">Artista desconocido</p>
                            
                            <div class="waveform mb-4 w-full"></div>
                            
                            <div class="flex justify-between w-full text-sm text-gray-400">
                                <span>0:00</span>
                                <span id="song-full-duration">3:45</span>
                            </div>
                        </div>
                        
                        <div class="space-y-4">
                            <div>
                                <h4 class="text-sm font-medium text-gray-300 mb-1">Género principal</h4>
                                <div class="flex items-center">
                                    <span id="song-genre" class="bg-indigo-900 text-indigo-300 px-3 py-1 rounded-full text-sm">Pop</span>
                                    <span class="ml-auto text-indigo-400 text-sm">78%</span>
                                </div>
                            </div>
                            
                            <div>
                                <h4 class="text-sm font-medium text-gray-300 mb-1">BPM</h4>
                                <div class="flex items-center">
                                    <span id="song-bpm" class="bg-purple-900 text-purple-300 px-3 py-1 rounded-full text-sm">120 BPM</span>
                                </div>
                            </div>
                            
                            <div>
                                <h4 class="text-sm font-medium text-gray-300 mb-1">Tonalidad</h4>
                                <div class="flex items-center">
                                    <span id="song-key" class="bg-blue-900 text-blue-300 px-3 py-1 rounded-full text-sm">C Mayor</span>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Column 2: Instruments Timeline -->
                    <div class="glass-card rounded-xl p-6">
                        <h3 class="text-lg font-semibold text-indigo-300 mb-4">Línea de tiempo de instrumentos</h3>
                        
                        <div class="space-y-5">
                            <!-- Timeline header -->
                            <div class="flex justify-between text-xs text-gray-500 mb-1 px-2">
                                <span>0:00</span>
                                <span>1:00</span>
                                <span>2:00</span>
                                <span>3:00</span>
                            </div>
                            
                            <!-- Timeline tracks -->
                            <div class="space-y-4">
                                <!-- Guitar -->
                                <div>
                                    <div class="flex items-center mb-1">
                                        <i class="fas fa-guitar text-indigo-400 mr-2"></i>
                                        <span class="text-sm font-medium text-gray-300">Guitarra</span>
                                        <span class="ml-auto text-xs text-gray-500">85%</span>
                                    </div>
                                    <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                                        <div class="h-full bg-indigo-500 timeline-instrument" style="width: 85%;"></div>
                                    </div>
                                </div>
                                
                                <!-- Piano -->
                                <div>
                                    <div class="flex items-center mb-1">
                                        <i class="fas fa-piano-keyboard text-blue-400 mr-2"></i>
                                        <span class="text-sm font-medium text-gray-300">Piano</span>
                                        <span class="ml-auto text-xs text-gray-500">60%</span>
                                    </div>
                                    <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                                        <div class="h-full bg-blue-500 timeline-instrument" style="width: 60%;"></div>
                                    </div>
                                </div>
                                
                                <!-- Drums -->
                                <div>
                                    <div class="flex items-center mb-1">
                                        <i class="fas fa-drum text-purple-400 mr-2"></i>
                                        <span class="text-sm font-medium text-gray-300">Batería</span>
                                        <span class="ml-auto text-xs text-gray-500">95%</span>
                                    </div>
                                    <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                                        <div class="h-full bg-purple-500 timeline-instrument" style="width: 95%;"></div>
                                    </div>
                                </div>
                                
                                <!-- Bass -->
                                <div>
                                    <div class="flex items-center mb-1">
                                        <i class="fas fa-guitar text-green-400 mr-2"></i>
                                        <span class="text-sm font-medium text-gray-300">Bajo</span>
                                        <span class="ml-auto text-xs text-gray-500">90%</span>
                                    </div>
                                    <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                                        <div class="h-full bg-green-500 timeline-instrument" style="width: 90%;"></div>
                                    </div>
                                </div>
                                
                                <!-- Trumpet -->
                                <div>
                                    <div class="flex items-center mb-1">
                                        <i class="fas fa-trumpet text-yellow-400 mr-2"></i>
                                        <span class="text-sm font-medium text-gray-300">Trompeta</span>
                                        <span class="ml-auto text-xs text-gray-500">30%</span>
                                    </div>
                                    <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                                        <div class="h-full bg-yellow-500 timeline-instrument" style="width: 30%; margin-left: 50%;"></div>
                                    </div>
                                </div>
                                
                                <!-- Saxophone -->
                                <div>
                                    <div class="flex items-center mb-1">
                                        <i class="fas fa-saxophone text-red-400 mr-2"></i>
                                        <span class="text-sm font-medium text-gray-300">Saxofón</span>
                                        <span class="ml-auto text-xs text-gray-500">25%</span>
                                    </div>
                                    <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                                        <div class="h-full bg-red-500 timeline-instrument" style="width: 25%; margin-left: 60%;"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Column 3: Detailed Analysis -->
                    <div class="glass-card rounded-xl p-6">
                        <h3 class="text-lg font-semibold text-indigo-300 mb-4">Análisis Detallado</h3>
                        
                        <!-- Genre Breakdown -->
                        <div class="mb-6">
                            <h4 class="text-md font-medium text-gray-300 mb-3">Distribución de Géneros</h4>
                            <div class="space-y-2">
                                <div>
                                    <div class="flex justify-between text-sm mb-1">
                                        <span class="text-gray-400">Pop</span>
                                        <span class="text-indigo-400">78%</span>
                                    </div>
                                    <div class="w-full bg-gray-800 rounded-full h-1.5">
                                        <div class="bg-indigo-500 h-1.5 rounded-full" style="width: 78%"></div>
                                    </div>
                                </div>
                                <div>
                                    <div class="flex justify-between text-sm mb-1">
                                        <span class="text-gray-400">Rock</span>
                                        <span class="text-blue-400">45%</span>
                                    </div>
                                    <div class="w-full bg-gray-800 rounded-full h-1.5">
                                        <div class="bg-blue-500 h-1.5 rounded-full" style="width: 45%"></div>
                                    </div>
                                </div>
                                <div>
                                    <div class="flex justify-between text-sm mb-1">
                                        <span class="text-gray-400">Jazz</span>
                                        <span class="text-purple-400">32%</span>
                                    </div>
                                    <div class="w-full bg-gray-800 rounded-full h-1.5">
                                        <div class="bg-purple-500 h-1.5 rounded-full" style="width: 32%"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Song Structure -->
                        <div class="mb-6">
                            <h4 class="text-md font-medium text-gray-300 mb-3">Estructura Musical</h4>
                            <div class="flex overflow-x-auto pb-2 space-x-2">
                                <div class="flex flex-col items-center bg-indigo-900/50 px-3 py-2 rounded-lg min-w-[70px]">
                                    <span class="text-xs text-indigo-300">Intro</span>
                                    <span class="text-xs text-gray-400">0:00-0:30</span>
                                </div>
                                <div class="flex flex-col items-center bg-blue-900/50 px-3 py-2 rounded-lg min-w-[70px]">
                                    <span class="text-xs text-blue-300">Estrofa</span>
                                    <span class="text-xs text-gray-400">0:30-0:58</span>
                                </div>
                                <div class="flex flex-col items-center bg-purple-900/50 px-3 py-2 rounded-lg min-w-[70px]">
                                    <span class="text-xs text-purple-300">Coro</span>
                                    <span class="text-xs text-gray-400">0:58-1:26</span>
                                </div>
                                <div class="flex flex-col items-center bg-yellow-900/50 px-3 py-2 rounded-lg min-w-[70px]">
                                    <span class="text-xs text-yellow-300">Puente</span>
                                    <span class="text-xs text-gray-400">2:22-2:50</span>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Instrument Details -->
                        <div>
                            <h4 class="text-md font-medium text-gray-300 mb-3">Detalles de Instrumentos</h4>
                            <div class="space-y-3">
                                <div class="flex items-start">
                                    <i class="fas fa-guitar text-indigo-400 mt-1 mr-2"></i>
                                    <div>
                                        <p class="text-sm font-medium text-gray-300">Guitarra eléctrica</p>
                                        <p class="text-xs text-gray-500">Presente en 85% de la canción</p>
                                    </div>
                                </div>
                                <div class="flex items-start">
                                    <i class="fas fa-drum text-purple-400 mt-1 mr-2"></i>
                                    <div>
                                        <p class="text-sm font-medium text-gray-300">Batería</p>
                                        <p class="text-xs text-gray-500">Ritmo constante con breaks</p>
                                    </div>
                                </div>
                                <div class="flex items-start">
                                    <i class="fas fa-trumpet text-yellow-400 mt-1 mr-2"></i>
                                    <div>
                                        <p class="text-sm font-medium text-gray-300">Metales</p>
                                        <p class="text-xs text-gray-500">Aparición en puente (2:22-2:50)</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Actions -->
                <div class="flex justify-end mt-6">
                    <button class="bg-indigo-600 text-white px-6 py-2 rounded-lg hover:bg-indigo-700 transition mr-3">
                        <i class="fas fa-download mr-2"></i> Exportar
                    </button>
                    <button class="bg-gray-800 border border-gray-700 text-gray-300 px-6 py-2 rounded-lg hover:bg-gray-700 transition">
                        <i class="fas fa-redo mr-2"></i> Nueva Análisis
                    </button>
                </div>
            </div>
            
            <!-- Loading State -->
            <div id="loading-section" class="hidden glass-card rounded-xl p-8 text-center max-w-2xl mx-auto">
                <div class="animate-pulse flex flex-col items-center justify-center">
                    <div class="w-20 h-20 bg-indigo-900/50 rounded-full flex items-center justify-center mb-4">
                        <i class="fas fa-music text-indigo-400 text-3xl"></i>
                    </div>
                    <h3 class="text-xl font-medium text-indigo-300 mb-2">Analizando tu canción</h3>
                    <p class="text-gray-400 mb-6">Procesando información musical...</p>
                    <div class="w-full bg-gray-800 rounded-full h-2.5">
                        <div id="progress-bar" class="bg-indigo-500 h-2.5 rounded-full" style="width: 0%"></div>
                    </div>
                    <p id="progress-text" class="text-sm text-gray-500 mt-2">0% completado</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Simulate file upload and analysis
        document.getElementById('file-upload').addEventListener('change', function(e) {
            // Show loading state
            document.getElementById('loading-section').classList.remove('hidden');
            
            // Simulate analysis progress
            let progress = 0;
            const progressBar = document.getElementById('progress-bar');
            const progressText = document.getElementById('progress-text');
            
            const interval = setInterval(() => {
                progress += Math.random() * 10;
                if (progress > 100) progress = 100;
                
                progressBar.style.width = `${progress}%`;
                progressText.textContent = `${Math.round(progress)}% completado`;
                
                if (progress === 100) {
                    clearInterval(interval);
                    
                    // Hide loading, show results after a delay
                    setTimeout(() => {
                        document.getElementById('loading-section').classList.add('hidden');
                        document.getElementById('results-section').classList.remove('hidden');
                        
                        // Update song info with dummy data
                        const file = e.target.files[0];
                        if (file) {
                            document.getElementById('song-title').textContent = file.name.replace(/\.[^/.]+$/, "");
                            document.getElementById('song-artist').textContent = "Artista desconocido";
                        }
                    }, 500);
                }
            }, 300);
        });
        
        // Simulate URL analysis
        document.querySelector('button').addEventListener('click', function() {
            const urlInput = document.querySelector('input[type="text"]');
            if (urlInput.value.trim() !== '') {
                // Show loading state
                document.getElementById('loading-section').classList.remove('hidden');
                
                // Update song info based on URL (simulated)
                document.getElementById('song-title').textContent = "Canción de ejemplo";
                document.getElementById('song-artist').textContent = "Artista popular";
                
                // Simulate analysis progress
                let progress = 0;
                const progressBar = document.getElementById('progress-bar');
                const progressText = document.getElementById('progress-text');
                
                const interval = setInterval(() => {
                    progress += Math.random() * 15;
                    if (progress > 100) progress = 100;
                    
                    progressBar.style.width = `${progress}%`;
                    progressText.textContent = `${Math.round(progress)}% completado`;
                    
                    if (progress === 100) {
                        clearInterval(interval);
                        
                        // Hide loading, show results after a delay
                        setTimeout(() => {
                            document.getElementById('loading-section').classList.add('hidden');
                            document.getElementById('results-section').classList.remove('hidden');
                        }, 500);
                    }
                }, 300);
            }
        });
    </script>
</body>
</html>
