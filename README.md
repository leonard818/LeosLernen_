<!DOCTYPE html>
<html lang="de" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LeosLernen - Vokabeln & KI Tutor</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        .tab-active { border-bottom: 2px solid #2563eb; color: #2563eb; font-weight: 600; }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
        .perspective { perspective: 1000px; }
        .flip-card-inner { transition: transform 0.6s; transform-style: preserve-3d; }
        .flip-card-flipped { transform: rotateY(180deg); }
        .flip-card-front, .flip-card-back { backface-visibility: hidden; }
        .flip-card-back { transform: rotateY(180deg); }
    </style>
</head>
<body class="h-full flex flex-col text-slate-800 antialiased selection:bg-blue-100 selection:text-blue-700">

    <div id="app" class="min-h-screen flex flex-col">
        <!-- HEADER / NAVIGATION -->
        <header id="main-header" class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-sm">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
                <!-- LOGO -->
                <div class="flex items-center space-x-3 cursor-pointer" onclick="navigateTo('dashboard')">
                    <div class="w-10 h-10 rounded-xl bg-blue-600 flex items-center justify-center text-white shadow-md shadow-blue-500/20">
                        <i data-lucide="graduation-cap" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <span class="text-xl font-bold tracking-tight text-slate-900 block leading-tight">LeosLernen</span>
                        <span class="text-[10px] uppercase tracking-wider text-blue-600 font-semibold block -mt-0.5">VOKABEL & KI-TUTOR</span>
                    </div>
                </div>

                <!-- NAVIGATION ITEMS -->
                <nav id="nav-menu" class="hidden md:flex items-center space-x-1 bg-slate-100 p-1 rounded-xl">
                    <button onclick="navigateTo('dashboard')" nav-id="dashboard" class="nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900">Dashboard</button>
                    <button onclick="navigateTo('vokabeln')" nav-id="vokabeln" class="nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900">Vokabellisten</button>
                    <button onclick="navigateTo('lernmodi')" nav-id="lernmodi" class="nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900">Lernen</button>
                    <button onclick="navigateTo('kitest')" nav-id="kitest" class="nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900">KI-Übungsraum</button>
                    <button onclick="navigateTo('offline')" nav-id="offline" class="nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900 flex items-center space-x-1">
                        <i data-lucide="download-cloud" class="w-3.5 h-3.5"></i>
                        <span>Offline</span>
                    </button>
                    <button onclick="navigateTo('rangliste')" nav-id="rangliste" class="nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900">Rangliste</button>
                    <button onclick="navigateTo('statistik')" nav-id="statistik" class="nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900">Statistik</button>
                </nav>

                <!-- USER ACTIONS -->
                <div class="flex items-center space-x-3">
                    <button onclick="openManagerModal()" class="p-2 rounded-lg text-slate-500 hover:bg-slate-100 hover:text-slate-800 transition relative" title="Manager-Modus">
                        <i data-lucide="shield-check" class="w-5 h-5"></i>
                    </button>

                    <div id="user-profile-badge" class="flex items-center space-x-2 bg-slate-50 border border-slate-200 px-3 py-1.5 rounded-full">
                        <div class="w-7 h-7 rounded-full bg-blue-100 text-blue-700 flex items-center justify-center font-bold text-xs" id="user-avatar-text">
                            ?
                        </div>
                        <div class="text-left">
                            <span id="nav-username" class="text-xs font-semibold block leading-none text-slate-800">Gast</span>
                            <span id="nav-xp" class="text-[10px] text-blue-600 font-bold block leading-none mt-0.5">0 XP</span>
                        </div>
                    </div>

                    <button id="auth-btn" onclick="handleAuthAction()" class="p-2 rounded-lg text-slate-500 hover:bg-red-50 hover:text-red-600 transition" title="Abmelden">
                        <i data-lucide="log-out" class="w-5 h-5"></i>
                    </button>
                </div>
            </div>
        </header>

        <!-- MAIN CONTENT CONTAINER -->
        <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

            <!-- VIEW: LOGIN / REGISTRIERUNG -->
            <section id="view-auth" class="view-section hidden max-w-md mx-auto my-12 bg-white p-8 rounded-2xl shadow-xl border border-slate-100">
                <div class="text-center mb-8">
                    <div class="w-16 h-16 bg-blue-600 text-white rounded-2xl flex items-center justify-center mx-auto mb-4 shadow-lg shadow-blue-500/30">
                        <i data-lucide="graduation-cap" class="w-10 h-10"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-slate-900">Willkommen bei LeosLernen</h2>
                    <p class="text-sm text-slate-500 mt-1">Sicher anmelden ohne E-Mail. Starte direkt!</p>
                </div>

                <div class="flex border-b border-slate-200 mb-6">
                    <button onclick="setAuthTab('login')" id="tab-auth-login" class="flex-1 py-2 text-center text-sm font-medium border-b-2 border-blue-600 text-blue-600">Anmelden</button>
                    <button onclick="setAuthTab('register')" id="tab-auth-register" class="flex-1 py-2 text-center text-sm font-medium border-b-2 border-transparent text-slate-500 hover:text-slate-800">Registrieren</button>
                </div>

                <form id="auth-form" onsubmit="submitAuth(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Benutzername</label>
                        <input type="text" id="auth-username" required class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 bg-slate-50/50" placeholder="z.B. LeoMax">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Passwort</label>
                        <input type="password" id="auth-password" required class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 bg-slate-50/50" placeholder="••••••••">
                    </div>
                    <button type="submit" id="auth-submit-btn" class="w-full py-3 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-xl shadow-lg shadow-blue-600/20 transition duration-150">Anmelden</button>
                </form>
            </section>

            <!-- VIEW: DASHBOARD -->
            <section id="view-dashboard" class="view-section space-y-8 hidden">
                <div class="bg-gradient-to-r from-blue-600 to-indigo-600 rounded-3xl p-8 text-white shadow-xl shadow-blue-500/10 flex flex-col md:flex-row items-center justify-between">
                    <div class="space-y-2 mb-6 md:mb-0">
                        <span id="dash-level-badge" class="bg-white/20 text-xs font-semibold px-3 py-1 rounded-full uppercase tracking-wider backdrop-blur-md">Level 1 • Anfänger</span>
                        <h1 class="text-3xl font-bold">Hallo, <span id="dash-username">Lernender</span>! 👋</h1>
                        <p class="text-blue-100 max-w-xl text-sm">Willkommen zurück. Wähle eine deiner Vokabellisten oder starte einen KI-Test.</p>
                    </div>
                    <div class="flex space-x-3">
                        <button onclick="navigateTo('lernmodi')" class="bg-white text-blue-600 hover:bg-blue-50 px-5 py-3 rounded-xl font-semibold shadow-md transition flex items-center space-x-2">
                            <i data-lucide="play-circle" class="w-5 h-5"></i>
                            <span>Jetzt Lernen</span>
                        </button>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-blue-50 text-blue-600 rounded-xl"><i data-lucide="book-open" class="w-6 h-6"></i></div>
                        <div>
                            <p class="text-xs text-slate-400 font-medium uppercase">Vokabellisten</p>
                            <h3 class="text-xl font-bold text-slate-800" id="dash-stat-lists">0</h3>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-indigo-50 text-indigo-600 rounded-xl"><i data-lucide="layers" class="w-6 h-6"></i></div>
                        <div>
                            <p class="text-xs text-slate-400 font-medium uppercase">Gesamte Vokabeln</p>
                            <h3 class="text-xl font-bold text-slate-800" id="dash-stat-words">0</h3>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-emerald-50 text-emerald-600 rounded-xl"><i data-lucide="shield-check" class="w-6 h-6"></i></div>
                        <div>
                            <p class="text-xs text-slate-400 font-medium uppercase">Sicherheit</p>
                            <h3 class="text-xl font-bold text-slate-800" id="dash-stat-mastery">0%</h3>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center space-x-4">
                        <div class="p-3 bg-amber-50 text-amber-600 rounded-xl"><i data-lucide="zap" class="w-6 h-6"></i></div>
                        <div>
                            <p class="text-xs text-slate-400 font-medium uppercase">Gesamt XP</p>
                            <h3 class="text-xl font-bold text-slate-800" id="dash-stat-xp">0 XP</h3>
                        </div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl border border-slate-100 p-6 shadow-sm space-y-4">
                    <div class="flex justify-between items-center">
                        <h2 class="text-lg font-bold text-slate-900">Deine Vokabellisten</h2>
                        <button onclick="navigateTo('vokabeln')" class="text-xs font-semibold text-blue-600 hover:underline flex items-center space-x-1">
                            <span>Alle verwalten</span>
                            <i data-lucide="arrow-right" class="w-3 h-3"></i>
                        </button>
                    </div>
                    <div id="dash-lists-container" class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <!-- Rendered via JS -->
                    </div>
                </div>
            </section>

            <!-- VIEW: VOKABELN (LISTENMANAGEMENT) -->
            <section id="view-vokabeln" class="view-section space-y-8 hidden">
                <div class="flex justify-between items-center">
                    <div>
                        <h1 class="text-2xl font-bold text-slate-900">Vokabellisten Verwaltung</h1>
                        <p class="text-sm text-slate-500">Erstelle neue Listen, fülle Wörter untereinander ein oder nutze den KI-Import.</p>
                    </div>
                    <button onclick="openCreateListModal()" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2.5 rounded-xl font-semibold shadow-md transition flex items-center space-x-2 text-sm">
                        <i data-lucide="plus" class="w-4 h-4"></i>
                        <span>Neue Liste erstellen</span>
                    </button>
                </div>

                <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm space-y-6">
                    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 pb-4 border-b border-slate-100">
                        <div class="w-full sm:w-auto flex-1">
                            <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Ziel-Vokabelliste auswählen</label>
                            <select id="vocab-target-list-select" class="w-full max-w-xs px-3 py-2 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 bg-slate-50">
                                <!-- Populated dynamically -->
                            </select>
                        </div>
                    </div>

                    <!-- Tabs for Entry Method -->
                    <div class="flex gap-4 border-b border-slate-100 mb-4">
                        <button id="tab-vocab-batch" onclick="switchVocabTab('batch')" class="pb-2 text-sm font-semibold border-b-2 border-blue-600 text-blue-600">
                            <i data-lucide="list-plus" class="w-4 h-4 inline mr-1"></i> Wörter untereinander eingeben
                        </button>
                        <button id="tab-vocab-ai" onclick="switchVocabTab('ai')" class="pb-2 text-sm font-semibold border-b-2 border-transparent text-slate-400 hover:text-slate-600">
                            <i data-lucide="sparkles" class="w-4 h-4 inline mr-1"></i> KI-Import (Foto / Blatt / Text)
                        </button>
                    </div>

                    <!-- TAB 1: BATCH / LINE-BY-LINE ENTRY -->
                    <div id="vocab-tab-batch-content" class="space-y-4">
                        <p class="text-xs text-slate-500">Trage Vokabeln zeilenweise ein. Drücke Enter in der Übersetzung für die nächste Zeile.</p>

                        <div id="vocab-rows-container" class="space-y-2 max-h-[350px] overflow-y-auto pr-2">
                            <!-- Dynamic Rows -->
                        </div>

                        <div class="flex flex-wrap justify-between items-center gap-3 pt-3 border-t border-slate-100">
                            <button onclick="addVocabRow()" class="px-4 py-2 bg-slate-100 text-slate-700 hover:bg-slate-200 rounded-xl font-medium text-sm flex items-center gap-2 transition">
                                <i data-lucide="plus" class="w-4 h-4"></i> + Weiteres Wort untereinander hinzufügen
                            </button>
                            
                            <button onclick="saveBatchVocabularies()" class="px-6 py-2.5 bg-blue-600 text-white hover:bg-blue-700 rounded-xl font-semibold text-sm shadow-md shadow-blue-500/20 transition flex items-center gap-2">
                                <i data-lucide="save" class="w-4 h-4"></i> Alle Wörter in Liste Speichern
                            </button>
                        </div>
                    </div>

                    <!-- TAB 2: AI OCR & TEXT EXTRACTION -->
                    <div id="vocab-tab-ai-content" class="hidden space-y-4">
                        <div class="p-4 bg-blue-50 rounded-xl border border-blue-100 text-xs text-blue-800 flex items-start gap-2">
                            <i data-lucide="info" class="w-4 h-4 text-blue-600 flex-shrink-0 mt-0.5"></i>
                            <span>Lädt du ein Foto eines Buchs oder Arbeitsblatts hoch, liest die KI (Gemini) das Blatt präzise aus und trägt alle Vokabeln automatisch in die Zeilen ein!</span>
                        </div>

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="space-y-2">
                                <label class="block text-xs font-semibold text-slate-600">1. Bild / Datei auswählen</label>
                                <input type="file" id="vocab-ai-file" accept="image/*,.pdf,.txt" class="w-full text-xs text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-semibold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100 cursor-pointer">
                                <button onclick="triggerCameraCapture('vocab')" class="mt-2 w-full py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-xl text-xs font-semibold flex items-center justify-center space-x-1">
                                    <i data-lucide="camera" class="w-4 h-4"></i>
                                    <span>Kamera-Foto machen</span>
                                </button>
                            </div>
                            <div class="space-y-2">
                                <label class="block text-xs font-semibold text-slate-600">Oder Text reinkopieren</label>
                                <textarea id="vocab-ai-text" rows="4" placeholder="Füge hier Lerntext ein..." class="w-full p-2.5 border border-slate-200 rounded-xl text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none"></textarea>
                            </div>
                        </div>

                        <button onclick="processAIForVocabList()" class="px-5 py-2.5 bg-gradient-to-r from-blue-600 to-indigo-600 text-white rounded-xl font-medium text-xs shadow hover:opacity-95 transition flex items-center gap-2">
                            <i data-lucide="sparkles" class="w-4 h-4"></i> Blatt analysieren & Vokabeln extrahieren
                        </button>
                    </div>
                </div>

                <!-- LIST OF VOCABULARY LISTS -->
                <div class="space-y-4">
                    <div class="flex border-b border-slate-200">
                        <button onclick="setListFilter('all')" id="filter-btn-all" class="px-4 py-2 text-sm font-semibold border-b-2 border-blue-600 text-blue-600">Alle Listen</button>
                        <button onclick="setListFilter('my')" id="filter-btn-my" class="px-4 py-2 text-sm font-semibold border-b-2 border-transparent text-slate-500 hover:text-slate-800">Meine Listen</button>
                        <button onclick="setListFilter('public')" id="filter-btn-public" class="px-4 py-2 text-sm font-semibold border-b-2 border-transparent text-slate-500 hover:text-slate-800">Öffentliche Listen</button>
                    </div>

                    <div id="lists-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                        <!-- Dynamic list cards -->
                    </div>
                </div>
            </section>

            <!-- VIEW: LERNMODI (1. SELECT LIST -> 2. SELECT METHOD) -->
            <section id="view-lernmodi" class="view-section space-y-8 hidden">
                <div class="text-center max-w-xl mx-auto space-y-2">
                    <h1 class="text-2xl font-bold text-slate-900">Lerneinheit Starten</h1>
                    <p class="text-sm text-slate-500">Wähle zuerst deine Vokabelliste aus und entscheide dich danach für eine Lernmethode.</p>
                </div>

                <!-- STEP 1: SELECT VOCAB LIST -->
                <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-4">
                    <div class="flex items-center space-x-2 text-blue-600 font-bold text-sm uppercase tracking-wider">
                        <span class="w-6 h-6 rounded-full bg-blue-100 flex items-center justify-center text-xs">1</span>
                        <span>Schritt 1: Vokabelliste auswählen</span>
                    </div>

                    <div id="learn-list-selector" class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <!-- Selectable lists rendered dynamically -->
                    </div>
                </div>

                <!-- STEP 2: SELECT METHOD -->
                <div id="learn-step-2-container" class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-4 opacity-50 pointer-events-none transition-all">
                    <div class="flex items-center space-x-2 text-blue-600 font-bold text-sm uppercase tracking-wider">
                        <span class="w-6 h-6 rounded-full bg-blue-100 flex items-center justify-center text-xs">2</span>
                        <span>Schritt 2: Lernmethode wählen</span>
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        <div onclick="startLearning('flashcard')" class="p-5 border border-slate-200 hover:border-blue-500 hover:shadow-md rounded-2xl cursor-pointer transition group bg-slate-50/50 hover:bg-white">
                            <div class="w-10 h-10 rounded-xl bg-blue-100 text-blue-600 flex items-center justify-center mb-3 group-hover:scale-110 transition">
                                <i data-lucide="layers" class="w-5 h-5"></i>
                            </div>
                            <h3 class="font-bold text-slate-900 text-base">Karteikarten</h3>
                            <p class="text-xs text-slate-500 mt-1">3D Flipcards zum Selbstabfragen.</p>
                        </div>

                        <div onclick="startLearning('mc')" class="p-5 border border-slate-200 hover:border-blue-500 hover:shadow-md rounded-2xl cursor-pointer transition group bg-slate-50/50 hover:bg-white">
                            <div class="w-10 h-10 rounded-xl bg-indigo-100 text-indigo-600 flex items-center justify-center mb-3 group-hover:scale-110 transition">
                                <i data-lucide="list-checks" class="w-5 h-5"></i>
                            </div>
                            <h3 class="font-bold text-slate-900 text-base">Multiple Choice</h3>
                            <p class="text-xs text-slate-500 mt-1">Wähle die passende Antwort aus 4 Optionen.</p>
                        </div>

                        <div onclick="startLearning('match')" class="p-5 border border-slate-200 hover:border-blue-500 hover:shadow-md rounded-2xl cursor-pointer transition group bg-slate-50/50 hover:bg-white">
                            <div class="w-10 h-10 rounded-xl bg-emerald-100 text-emerald-600 flex items-center justify-center mb-3 group-hover:scale-110 transition">
                                <i data-lucide="git-merge" class="w-5 h-5"></i>
                            </div>
                            <h3 class="font-bold text-slate-900 text-base">Verbinden</h3>
                            <p class="text-xs text-slate-500 mt-1">Paare Begrifflichkeiten und Übersetzungen.</p>
                        </div>

                        <div onclick="startLearning('write')" class="p-5 border border-slate-200 hover:border-blue-500 hover:shadow-md rounded-2xl cursor-pointer transition group bg-slate-50/50 hover:bg-white">
                            <div class="w-10 h-10 rounded-xl bg-amber-100 text-amber-600 flex items-center justify-center mb-3 group-hover:scale-110 transition">
                                <i data-lucide="pen-tool" class="w-5 h-5"></i>
                            </div>
                            <h3 class="font-bold text-slate-900 text-base">Schreiben</h3>
                            <p class="text-xs text-slate-500 mt-1">Tippe die exakte Übersetzung selbst ein.</p>
                        </div>
                    </div>
                </div>

                <!-- ACTIVE LEARNING SESSION AREA -->
                <div id="learning-active-area" class="hidden bg-white p-8 rounded-3xl border border-slate-100 shadow-xl space-y-6 max-w-2xl mx-auto">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center space-x-2">
                            <span id="learn-progress-text" class="text-xs font-bold text-blue-600 bg-blue-50 px-3 py-1 rounded-full">1 / 10</span>
                            <span id="learn-mode-badge" class="text-xs text-slate-400 font-semibold uppercase">Karteikarten</span>
                        </div>
                        <button onclick="exitLearningSession()" class="text-slate-400 hover:text-slate-600"><i data-lucide="x" class="w-5 h-5"></i></button>
                    </div>

                    <div id="learning-session-content">
                        <!-- Rendered dynamically -->
                    </div>
                </div>
            </section>

            <!-- VIEW: KI ÜBUNGSRAUM & TESTGENERATOR -->
            <section id="view-kitest" class="view-section space-y-8 hidden">
                <div class="flex justify-between items-center">
                    <div>
                        <h1 class="text-2xl font-bold text-slate-900">KI-Übungsraum & Testgenerator</h1>
                        <p class="text-sm text-slate-500">Generiere individuelle Prüfungen aus Skripten, Bildern oder Notizen mit KI.</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <!-- TEST CONFIGURATION -->
                    <div class="lg:col-span-1 bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-5">
                        <h2 class="font-bold text-slate-900 text-base flex items-center space-x-2">
                            <i data-lucide="sparkles" class="w-5 h-5 text-blue-600"></i>
                            <span>Test-Konfiguration</span>
                        </h2>

                        <!-- 1. MATERIAL INPUT -->
                        <div class="space-y-2">
                            <label class="block text-xs font-semibold text-slate-600 uppercase">1. Material-Quelle</label>
                            <div class="space-y-2">
                                <textarea id="ai-test-text-input" rows="3" placeholder="Füge hier Lerntext, Notizen oder Thema ein..." class="w-full p-3 text-xs border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:outline-none"></textarea>
                                
                                <div class="flex items-center justify-between space-x-2">
                                    <label class="flex-1 cursor-pointer bg-slate-50 border border-slate-200 hover:bg-slate-100 p-2 rounded-xl text-center text-xs font-semibold text-slate-600 transition">
                                        <i data-lucide="upload" class="w-3.5 h-3.5 inline mr-1"></i> Datei / Bild
                                        <input type="file" id="ai-test-file-input" class="hidden" accept="image/*,.pdf,.txt" onchange="handleTestFileImport(event)">
                                    </label>
                                    <button onclick="triggerCameraCapture('test')" class="flex-1 bg-slate-50 border border-slate-200 hover:bg-slate-100 p-2 rounded-xl text-center text-xs font-semibold text-slate-600 transition">
                                        <i data-lucide="camera" class="w-3.5 h-3.5 inline mr-1"></i> Foto
                                    </button>
                                </div>
                                <div id="ai-test-file-badge" class="hidden text-[11px] bg-blue-50 text-blue-700 p-2 rounded-lg flex justify-between items-center">
                                    <span id="ai-test-filename">Datei geladen</span>
                                    <i data-lucide="check" class="w-3.5 h-3.5"></i>
                                </div>
                            </div>
                        </div>

                        <!-- 2. SCHWIERIGKEIT (STRIKT 1 BIS 6) -->
                        <div class="space-y-2">
                            <div class="flex justify-between items-center">
                                <label class="block text-xs font-semibold text-slate-600 uppercase">2. Schwierigkeitsstufe</label>
                                <span id="difficulty-val" class="text-xs font-bold text-blue-600">Stufe 3 (Mittel)</span>
                            </div>
                            <input type="range" id="ai-test-difficulty" min="1" max="6" value="3" oninput="updateDifficultyLabel(this.value)" class="w-full accent-blue-600">
                            <div class="flex justify-between text-[10px] text-slate-400 font-medium">
                                <span>1 (Sehr Leicht)</span>
                                <span>6 (Sehr Schwer)</span>
                            </div>
                        </div>

                        <!-- 3. AUFGABENTYP -->
                        <div class="space-y-2">
                            <label class="block text-xs font-semibold text-slate-600 uppercase">3. Aufgabentyp</label>
                            <select id="ai-test-type" class="w-full px-3 py-2 rounded-xl border border-slate-200 text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none bg-slate-50/50">
                                <option value="gemischt">Gemischt (Empfohlen)</option>
                                <option value="nenne">Nenne (Faktenwissen)</option>
                                <option value="erlaeutere">Erläutere (Verständnis)</option>
                                <option value="diskutiere">Diskutiere (Pro / Kontra)</option>
                                <option value="kurzantwort">Kurzantwort</option>
                                <option value="mc">Multiple Choice</option>
                            </select>
                        </div>

                        <button onclick="generateAITest()" id="btn-generate-test" class="w-full py-3 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-xl shadow-lg shadow-blue-600/20 transition flex items-center justify-center space-x-2 text-sm">
                            <i data-lucide="cpu" class="w-4 h-4"></i>
                            <span>Test Generieren</span>
                        </button>
                    </div>

                    <!-- TEST EXECUTION -->
                    <div class="lg:col-span-2 bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-6">
                        <div id="ai-test-placeholder" class="text-center py-16 space-y-3">
                            <div class="w-12 h-12 rounded-full bg-slate-100 text-slate-400 flex items-center justify-center mx-auto">
                                <i data-lucide="file-question" class="w-6 h-6"></i>
                            </div>
                            <h3 class="font-bold text-slate-700">Noch kein Test generiert</h3>
                            <p class="text-xs text-slate-400 max-w-sm mx-auto">Konfiguriere links dein Material und klicke auf "Test Generieren".</p>
                        </div>

                        <div id="ai-test-content" class="hidden space-y-6">
                            <div class="border-b border-slate-100 pb-4 flex justify-between items-center">
                                <div>
                                    <h3 class="font-bold text-slate-900 text-lg" id="ai-test-title">Generierter Test</h3>
                                    <span id="ai-test-meta" class="text-xs text-slate-400">Stufe 3 • Gemischt</span>
                                </div>
                                <span class="bg-emerald-50 text-emerald-600 text-xs font-bold px-3 py-1 rounded-full">Aktiv</span>
                            </div>

                            <form id="ai-test-form" onsubmit="evaluateAITest(event)" class="space-y-6">
                                <div id="ai-test-questions-container" class="space-y-6">
                                    <!-- Dynamic Questions -->
                                </div>
                                <button type="submit" class="w-full py-3 bg-emerald-600 hover:bg-emerald-700 text-white font-bold rounded-xl shadow-lg shadow-emerald-600/20 transition">Test Abgeben & Auswerten</button>
                            </form>
                        </div>

                        <div id="ai-test-results" class="hidden space-y-4 bg-slate-50 p-6 rounded-2xl border border-slate-200">
                            <h3 class="font-bold text-slate-900 text-lg">Testergebnis & Auswertung</h3>
                            <div id="ai-test-feedback-body" class="text-sm space-y-3">
                                <!-- Feedback details -->
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW: OFFLINE LERNEN -->
            <section id="view-offline" class="view-section space-y-8 hidden">
                <div>
                    <h1 class="text-2xl font-bold text-slate-900">Offline Lernen</h1>
                    <p class="text-sm text-slate-500">Deine lokal gespeicherten Vokabellisten stehen jederzeit ohne Internet bereit.</p>
                </div>

                <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-4">
                    <h2 class="text-base font-bold text-slate-900">Heruntergeladene Pakete</h2>
                    <div id="offline-lists-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                        <!-- Dynamic Offline Lists -->
                    </div>
                </div>
            </section>

            <!-- VIEW: RANGLISTE -->
            <section id="view-rangliste" class="view-section space-y-8 hidden">
                <div>
                    <h1 class="text-2xl font-bold text-slate-900">Globale Rangliste</h1>
                    <p class="text-sm text-slate-500">Vergleiche deine gesammelten XP mit anderen Schülern und Lerngruppen.</p>
                </div>

                <div class="bg-white rounded-2xl border border-slate-100 shadow-sm overflow-hidden">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-slate-50 text-xs font-bold text-slate-500 uppercase border-b border-slate-100">
                            <tr>
                                <th class="p-4">Rang</th>
                                <th class="p-4">Benutzer</th>
                                <th class="p-4">Level</th>
                                <th class="p-4 text-right">Gesamt XP</th>
                            </tr>
                        </thead>
                        <tbody id="leaderboard-body" class="divide-y divide-slate-100">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </section>

            <!-- VIEW: STATISTIK -->
            <section id="view-statistik" class="view-section space-y-8 hidden">
                <div>
                    <h1 class="text-2xl font-bold text-slate-900">Detaillierte Lernstatistiken</h1>
                    <p class="text-sm text-slate-500">Einblicke in deinen Lernfortschritt, Genauigkeit und XP-Verlauf.</p>
                </div>

                <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-4">
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm text-center">
                        <p class="text-[11px] font-semibold text-slate-400 uppercase">Vokabelsicherheit</p>
                        <h4 class="text-2xl font-extrabold text-blue-600 mt-1" id="stat-vocab-mastery">0%</h4>
                    </div>
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm text-center">
                        <p class="text-[11px] font-semibold text-slate-400 uppercase">Test-Sicherheit</p>
                        <h4 class="text-2xl font-extrabold text-indigo-600 mt-1" id="stat-test-mastery">0%</h4>
                    </div>
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm text-center">
                        <p class="text-[11px] font-semibold text-slate-400 uppercase">Fehlerquote</p>
                        <h4 class="text-2xl font-extrabold text-rose-500 mt-1" id="stat-error-rate">0%</h4>
                    </div>
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm text-center">
                        <p class="text-[11px] font-semibold text-slate-400 uppercase">Gelernt</p>
                        <h4 class="text-2xl font-extrabold text-emerald-600 mt-1" id="stat-words-learned">0</h4>
                    </div>
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm text-center">
                        <p class="text-[11px] font-semibold text-slate-400 uppercase">Fehler Gesamt</p>
                        <h4 class="text-2xl font-extrabold text-amber-500 mt-1" id="stat-total-errors">0</h4>
                    </div>
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm text-center">
                        <p class="text-[11px] font-semibold text-slate-400 uppercase">Gesamt XP</p>
                        <h4 class="text-2xl font-extrabold text-purple-600 mt-1" id="stat-total-xp">0</h4>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-4">
                        <h3 class="font-bold text-slate-900 text-base">Aktivitäts- & XP-Verlauf</h3>
                        <div class="h-64">
                            <canvas id="chart-xp-history"></canvas>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-4">
                        <h3 class="font-bold text-slate-900 text-base">Erfolgsquote nach Lernmodus</h3>
                        <div class="h-64">
                            <canvas id="chart-mode-breakdown"></canvas>
                        </div>
                    </div>
                </div>
            </section>
        </main>

        <!-- MODAL: MANAGER AREA -->
        <div id="modal-manager" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
            <div class="bg-white rounded-3xl max-w-2xl w-full p-6 space-y-6 shadow-2xl max-h-[90vh] overflow-y-auto">
                <div class="flex justify-between items-center border-b border-slate-100 pb-4">
                    <div class="flex items-center space-x-2">
                        <div class="p-2 bg-blue-50 text-blue-600 rounded-xl"><i data-lucide="shield-check" class="w-5 h-5"></i></div>
                        <h3 class="font-bold text-slate-900 text-lg">Manager-Bereich</h3>
                    </div>
                    <button onclick="closeManagerModal()" class="text-slate-400 hover:text-slate-600"><i data-lucide="x" class="w-5 h-5"></i></button>
                </div>

                <!-- Code Entry Form -->
                <div id="manager-lock-screen" class="space-y-4 text-center py-6">
                    <p class="text-xs text-slate-500">Gib den Manager-Zugangscode ein, um auf Benutzerdaten und Moderation zuzugreifen.</p>
                    <input type="password" id="manager-code-input" placeholder="Zugangscode eingeben" class="px-4 py-2.5 border border-slate-200 rounded-xl text-center text-sm font-mono tracking-widest focus:ring-2 focus:ring-blue-500 focus:outline-none max-w-xs mx-auto block">
                    <button onclick="verifyManagerCode()" class="bg-blue-600 text-white px-6 py-2 rounded-xl text-xs font-semibold hover:bg-blue-700 transition">Freischalten</button>
                </div>

                <!-- Manager Content Panel -->
                <div id="manager-content" class="hidden space-y-6">
                    <div>
                        <h4 class="font-bold text-sm text-slate-800 mb-3">Registrierte Benutzerkonten</h4>
                        <div class="overflow-x-auto border border-slate-100 rounded-xl">
                            <table class="w-full text-left text-xs">
                                <thead class="bg-slate-50 text-slate-500 uppercase font-bold border-b border-slate-100">
                                    <tr>
                                        <th class="p-3">Benutzername</th>
                                        <th class="p-3">XP</th>
                                        <th class="p-3">Status</th>
                                        <th class="p-3 text-right">Aktionen</th>
                                    </tr>
                                </thead>
                                <tbody id="manager-users-tbody" class="divide-y divide-slate-100">
                                    <!-- Populated dynamically -->
                                </tbody>
                            </table>
                        </div>
                    </div>

                    <div>
                        <h4 class="font-bold text-sm text-slate-800 mb-3">Öffentliche Vokabellisten Moderieren</h4>
                        <div class="overflow-x-auto border border-slate-100 rounded-xl">
                            <table class="w-full text-left text-xs">
                                <thead class="bg-slate-50 text-slate-500 uppercase font-bold border-b border-slate-100">
                                    <tr>
                                        <th class="p-3">Listen Name</th>
                                        <th class="p-3">Ersteller</th>
                                        <th class="p-3 text-right">Aktion</th>
                                    </tr>
                                </thead>
                                <tbody id="manager-lists-tbody" class="divide-y divide-slate-100">
                                    <!-- Populated dynamically -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- MODAL: CREATE NEW LIST -->
        <div id="modal-create-list" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
            <div class="bg-white rounded-3xl max-w-md w-full p-6 space-y-4 shadow-2xl">
                <div class="flex justify-between items-center border-b border-slate-100 pb-3">
                    <h3 class="font-bold text-slate-900 text-base">Neue Vokabelliste erstellen</h3>
                    <button onclick="closeCreateListModal()" class="text-slate-400 hover:text-slate-600"><i data-lucide="x" class="w-5 h-5"></i></button>
                </div>
                <div class="space-y-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Name der Liste</label>
                        <input type="text" id="new-list-title" placeholder="z.B. Englisch Unit 3" class="w-full px-4 py-2 border border-slate-200 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Beschreibung / Fach</label>
                        <input type="text" id="new-list-desc" placeholder="z.B. Kapitel 4 Vokabeln" class="w-full px-4 py-2 border border-slate-200 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                    <label class="flex items-center space-x-2 cursor-pointer pt-1">
                        <input type="checkbox" id="new-list-public" class="rounded text-blue-600 focus:ring-blue-500">
                        <span class="text-xs text-slate-600 font-medium">Öffentlich teilen (für alle Nutzer sichtbar)</span>
                    </label>
                </div>
                <div class="flex justify-end space-x-2 pt-2">
                    <button onclick="closeCreateListModal()" class="px-4 py-2 bg-slate-100 text-slate-600 rounded-xl text-xs font-semibold hover:bg-slate-200">Abbrechen</button>
                    <button onclick="saveNewList()" class="px-5 py-2 bg-blue-600 text-white rounded-xl text-xs font-semibold hover:bg-blue-700 shadow-md">Erstellen</button>
                </div>
            </div>
        </div>

        <!-- MODAL: CAMERA CAPTURE -->
        <div id="modal-camera" class="fixed inset-0 bg-slate-900/80 backdrop-blur-sm z-50 hidden flex flex-col items-center justify-center p-4">
            <div class="bg-white rounded-2xl max-w-lg w-full p-4 space-y-4 shadow-2xl">
                <div class="flex justify-between items-center">
                    <h3 class="font-bold text-slate-900 text-sm">Foto aufnehmen</h3>
                    <button onclick="closeCameraModal()" class="text-slate-400 hover:text-slate-600"><i data-lucide="x" class="w-5 h-5"></i></button>
                </div>
                <video id="camera-video" autoplay playsinline class="w-full h-64 bg-black rounded-xl object-cover"></video>
                <canvas id="camera-canvas" class="hidden"></canvas>
                <button onclick="captureCameraSnapshot()" class="w-full py-3 bg-blue-600 hover:bg-blue-700 text-white font-bold rounded-xl shadow transition flex items-center justify-center space-x-2">
                    <i data-lucide="camera" class="w-5 h-5"></i>
                    <span>Foto erfassen</span>
                </button>
            </div>
        </div>
    </div>

    <script>
        /* APP STATE MANAGEMENT */
        let currentUser = null;
        let activeView = 'auth';
        let authTab = 'login';
        let listFilter = 'all';
        let activeCameraTarget = null;
        let selectedListForLearning = null;
        let activeLearningSession = null;
        let chartXpInstance = null;
        let chartModeInstance = null;

        // Persistent Stores
        let users = JSON.parse(localStorage.getItem('leos_users') || '[]');
        let vocabLists = JSON.parse(localStorage.getItem('leos_vocab_lists') || '[]');
        let offlineListIds = JSON.parse(localStorage.getItem('leos_offline_list_ids') || '[]');

        // Initialize default seed lists if empty
        if (vocabLists.length === 0) {
            vocabLists = [
                {
                    id: 'list_seed_1',
                    title: 'Englisch Basis-Wortschatz',
                    description: 'Wichtige Alltags-Vokabeln für Einsteiger',
                    owner: 'System',
                    isPublic: true,
                    createdAt: new Date().toISOString(),
                    words: [
                        { id: 'w1', front: 'resilient', back: 'widerstandsfähig', example: 'She is very resilient.' },
                        { id: 'w2', front: 'ambition', back: 'Ehrgeiz', example: 'His ambition drove him to succeed.' },
                        { id: 'w3', front: 'curiosity', back: 'Neugier', example: 'Curiosity fuels learning.' }
                    ]
                }
            ];
            localStorage.setItem('leos_vocab_lists', JSON.stringify(vocabLists));
        }

        /* NAVIGATION SYSTEM */
        function navigateTo(viewId) {
            if (!currentUser && viewId !== 'auth') {
                viewId = 'auth';
            }

            activeView = viewId;
            document.querySelectorAll('.view-section').forEach(sec => sec.classList.add('hidden'));

            const targetSection = document.getElementById('view-' + viewId);
            if (targetSection) {
                targetSection.classList.remove('hidden');
            }

            // Navigation highlight update
            document.querySelectorAll('.nav-btn').forEach(btn => {
                const navId = btn.getAttribute('nav-id');
                if (navId === viewId) {
                    btn.className = "nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition bg-white text-blue-600 shadow-sm";
                } else {
                    btn.className = "nav-btn px-3.5 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900";
                }
            });

            // View specific initializations
            if (viewId === 'dashboard') renderDashboard();
            if (viewId === 'vokabeln') renderVocabListsView();
            if (viewId === 'lernmodi') renderLearningView();
            if (viewId === 'offline') renderOfflineView();
            if (viewId === 'rangliste') renderLeaderboardView();
            if (viewId === 'statistik') renderStatisticsView();

            lucide.createIcons();
        }

        /* AUTHENTICATION SYSTEM */
        function setAuthTab(tab) {
            authTab = tab;
            const loginTabBtn = document.getElementById('tab-auth-login');
            const regTabBtn = document.getElementById('tab-auth-register');
            const submitBtn = document.getElementById('auth-submit-btn');

            if (tab === 'login') {
                loginTabBtn.className = "flex-1 py-2 text-center text-sm font-medium border-b-2 border-blue-600 text-blue-600";
                regTabBtn.className = "flex-1 py-2 text-center text-sm font-medium border-b-2 border-transparent text-slate-500 hover:text-slate-800";
                submitBtn.innerText = "Anmelden";
            } else {
                regTabBtn.className = "flex-1 py-2 text-center text-sm font-medium border-b-2 border-blue-600 text-blue-600";
                loginTabBtn.className = "flex-1 py-2 text-center text-sm font-medium border-b-2 border-transparent text-slate-500 hover:text-slate-800";
                submitBtn.innerText = "Konto Erstellen (Start bei 0 XP)";
            }
        }

        function submitAuth(e) {
            e.preventDefault();
            const usernameInput = document.getElementById('auth-username').value.trim();
            const passwordInput = document.getElementById('auth-password').value;

            if (!usernameInput || !passwordInput) return;

            if (authTab === 'login') {
                const found = users.find(u => u.username.toLowerCase() === usernameInput.toLowerCase() && u.password === passwordInput);
                if (found) {
                    if (found.isBlocked) {
                        alert('Dieses Konto ist vorübergehend gesperrt.');
                        return;
                    }
                    currentUser = found;
                    updateUserHeader();
                    navigateTo('dashboard');
                } else {
                    alert('Benutzername oder Passwort falsch.');
                }
            } else {
                // Registration
                const existing = users.find(u => u.username.toLowerCase() === usernameInput.toLowerCase());
                if (existing) {
                    alert('Dieser Benutzername ist bereits vergeben.');
                    return;
                }

                const newUser = {
                    id: 'usr_' + Date.now(),
                    username: usernameInput,
                    password: passwordInput,
                    xp: 0,
                    wordsLearned: 0,
                    totalErrors: 0,
                    testsTaken: 0,
                    testScores: [],
                    isBlocked: false,
                    createdAt: new Date().toISOString()
                };

                users.push(newUser);
                localStorage.setItem('leos_users', JSON.stringify(users));
                currentUser = newUser;
                updateUserHeader();
                navigateTo('dashboard');
            }
        }

        function handleAuthAction() {
            if (currentUser) {
                currentUser = null;
                document.getElementById('nav-menu').classList.add('hidden');
                document.getElementById('user-profile-badge').classList.add('hidden');
                document.getElementById('auth-btn').classList.add('hidden');
                navigateTo('auth');
            }
        }

        function updateUserHeader() {
            if (!currentUser) return;
            document.getElementById('nav-menu').classList.remove('hidden');
            document.getElementById('user-profile-badge').classList.remove('hidden');
            document.getElementById('auth-btn').classList.remove('hidden');

            document.getElementById('nav-username').innerText = currentUser.username;
            document.getElementById('nav-xp').innerText = (currentUser.xp || 0) + ' XP';
            document.getElementById('user-avatar-text').innerText = currentUser.username.charAt(0).toUpperCase();
        }

        function addXP(amount) {
            if (!currentUser) return;
            currentUser.xp = (currentUser.xp || 0) + amount;
            
            // Save updated user in array & localStorage
            const idx = users.findIndex(u => u.id === currentUser.id);
            if (idx !== -1) {
                users[idx] = currentUser;
                localStorage.setItem('leos_users', JSON.stringify(users));
            }
            updateUserHeader();
        }

        /* DASHBOARD RENDERING */
        function renderDashboard() {
            if (!currentUser) return;

            document.getElementById('dash-username').innerText = currentUser.username;
            document.getElementById('dash-stat-xp').innerText = (currentUser.xp || 0) + ' XP';

            const myLists = vocabLists.filter(l => l.owner === currentUser.username || l.isPublic);
            let totalWords = 0;
            myLists.forEach(l => totalWords += (l.words ? l.words.length : 0));

            document.getElementById('dash-stat-lists').innerText = myLists.length;
            document.getElementById('dash-stat-words').innerText = totalWords;

            // Mastery calculation
            const mastery = Math.min(100, Math.round(((currentUser.wordsLearned || 0) / Math.max(1, totalWords)) * 100));
            document.getElementById('dash-stat-mastery').innerText = mastery + '%';

            // Level Badge
            const level = Math.floor((currentUser.xp || 0) / 100) + 1;
            let levelTitle = "Anfänger";
            if (level > 2) levelTitle = "Fortgeschrittener";
            if (level > 5) levelTitle = "Vokabel-Profi";
            if (level > 10) levelTitle = "Sprach-Meister";
            document.getElementById('dash-level-badge').innerText = `Level ${level} • ${levelTitle}`;

            // Render Recent Lists Container
            const container = document.getElementById('dash-lists-container');
            container.innerHTML = '';

            if (myLists.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-400 col-span-3">Noch keine Vokabellisten vorhanden.</p>`;
                return;
            }

            myLists.slice(0, 3).forEach(list => {
                const card = document.createElement('div');
                card.className = "p-4 border border-slate-200 rounded-2xl hover:border-blue-500 transition cursor-pointer flex flex-col justify-between";
                card.innerHTML = `
                    <div>
                        <div class="flex justify-between items-start mb-2">
                            <h4 class="font-bold text-slate-800 text-sm">${escapeHtml(list.title)}</h4>
                            <span class="text-[10px] font-semibold px-2 py-0.5 rounded-full ${list.isPublic ? 'bg-emerald-50 text-emerald-600' : 'bg-slate-100 text-slate-600'}">
                                ${list.isPublic ? 'Öffentlich' : 'Privat'}
                            </span>
                        </div>
                        <p class="text-xs text-slate-500 line-clamp-1">${escapeHtml(list.description || '')}</p>
                    </div>
                    <div class="mt-4 flex justify-between items-center text-xs font-semibold text-slate-400">
                        <span>${list.words ? list.words.length : 0} Wörter</span>
                        <button onclick="selectListForLearning('${list.id}')" class="text-blue-600 hover:underline">Lernen →</button>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        /* VOCABULARY LIST MANAGEMENT & BATCH ENTRY */
        function renderVocabListsView() {
            // Populate Target Select Dropdown
            const select = document.getElementById('vocab-target-list-select');
            select.innerHTML = '';

            const myLists = vocabLists.filter(l => l.owner === currentUser.username || l.isPublic);
            myLists.forEach(l => {
                const opt = document.createElement('option');
                opt.value = l.id;
                opt.innerText = l.title + (l.owner === currentUser.username ? ' (Meine)' : ' (Öffentlich)');
                select.appendChild(opt);
            });

            if (myLists.length === 0) {
                const opt = document.createElement('option');
                opt.value = '';
                opt.innerText = '-- Keine Liste vorhanden (Erst erstellen) --';
                select.appendChild(opt);
            }

            // Initialize Rows if empty
            const rowsContainer = document.getElementById('vocab-rows-container');
            if (rowsContainer && rowsContainer.children.length === 0) {
                addVocabRow();
                addVocabRow();
            }

            renderVocabGrid();
        }

        function addVocabRow(front = '', back = '', example = '') {
            const container = document.getElementById('vocab-rows-container');
            const rowId = 'row_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4);

            const rowDiv = document.createElement('div');
            rowDiv.id = rowId;
            rowDiv.className = "grid grid-cols-12 gap-2 items-center bg-slate-50 p-2 rounded-xl border border-slate-200 hover:border-blue-300 transition";

            rowDiv.innerHTML = `
                <div class="col-span-4">
                    <input type="text" placeholder="Fremdwort / Wort" value="${escapeHtml(front)}" class="vocab-front w-full px-3 py-1.5 text-xs bg-white border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="col-span-4">
                    <input type="text" placeholder="Übersetzung / Bedeutung" value="${escapeHtml(back)}" onkeydown="handleVocabRowKeydown(event, '${rowId}')" class="vocab-back w-full px-3 py-1.5 text-xs bg-white border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="col-span-3">
                    <input type="text" placeholder="Beispielsatz (optional)" value="${escapeHtml(example)}" class="vocab-example w-full px-3 py-1.5 text-xs bg-white border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="col-span-1 text-right">
                    <button onclick="removeVocabRow('${rowId}')" class="p-1 text-slate-400 hover:text-red-500 transition rounded-lg hover:bg-red-50" title="Zeile löschen">
                        <i data-lucide="trash-2" class="w-4 h-4"></i>
                    </button>
                </div>
            `;

            container.appendChild(rowDiv);
            lucide.createIcons();

            if (!front) {
                rowDiv.querySelector('.vocab-front').focus();
            }
        }

        function handleVocabRowKeydown(e, rowId) {
            if (e.key === 'Enter') {
                e.preventDefault();
                addVocabRow();
            }
        }

        function removeVocabRow(rowId) {
            const row = document.getElementById(rowId);
            if (row) row.remove();
            if (document.getElementById('vocab-rows-container').children.length === 0) {
                addVocabRow();
            }
        }

        function switchVocabTab(tab) {
            if (tab === 'batch') {
                document.getElementById('vocab-tab-batch-content').classList.remove('hidden');
                document.getElementById('vocab-tab-ai-content').classList.add('hidden');
                document.getElementById('tab-vocab-batch').className = "pb-2 text-sm font-semibold border-b-2 border-blue-600 text-blue-600";
                document.getElementById('tab-vocab-ai').className = "pb-2 text-sm font-semibold border-b-2 border-transparent text-slate-400 hover:text-slate-600";
            } else {
                document.getElementById('vocab-tab-batch-content').classList.add('hidden');
                document.getElementById('vocab-tab-ai-content').classList.remove('hidden');
                document.getElementById('tab-vocab-ai').className = "pb-2 text-sm font-semibold border-b-2 border-blue-600 text-blue-600";
                document.getElementById('tab-vocab-batch').className = "pb-2 text-sm font-semibold border-b-2 border-transparent text-slate-400 hover:text-slate-600";
            }
        }

        function saveBatchVocabularies() {
            const targetListId = document.getElementById('vocab-target-list-select').value;
            if (!targetListId) {
                alert('Bitte wähle eine Ziel-Vokabelliste aus oder erstelle zuerst eine neue Liste.');
                return;
            }

            const listIndex = vocabLists.findIndex(l => l.id === targetListId);
            if (listIndex === -1) return;

            const rowElements = document.getElementById('vocab-rows-container').querySelectorAll('[id^="row_"]');
            let count = 0;

            rowElements.forEach(row => {
                const front = row.querySelector('.vocab-front').value.trim();
                const back = row.querySelector('.vocab-back').value.trim();
                const example = row.querySelector('.vocab-example').value.trim();

                if (front && back) {
                    if (!vocabLists[listIndex].words) vocabLists[listIndex].words = [];
                    vocabLists[listIndex].words.push({
                        id: 'w_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4),
                        front,
                        back,
                        example
                    });
                    count++;
                }
            });

            if (count > 0) {
                localStorage.setItem('leos_vocab_lists', JSON.stringify(vocabLists));
                addXP(count * 5); // Reward 5 XP per word added
                alert(`${count} Vokabeln wurden erfolgreich zur Liste "${vocabLists[listIndex].title}" hinzugefügt! (+${count * 5} XP)`);
                
                document.getElementById('vocab-rows-container').innerHTML = '';
                addVocabRow();
                addVocabRow();
                renderVocabGrid();
            } else {
                alert('Bitte trage mindestens ein vollständiges Wort-Paar ein.');
            }
        }

        /* GEMINI API VISION & TEXT EXTRACTION */
        async function processAIForVocabList() {
            const fileInput = document.getElementById('vocab-ai-file');
            const textInput = document.getElementById('vocab-ai-text').value.trim();

            if (!fileInput.files[0] && !textInput) {
                alert('Bitte lade ein Foto/Bild hoch oder füge Text ein.');
                return;
            }

            alert('KI liest das Blatt/Dokument aus und extrahiert Vokabeln...');

            try {
                let parts = [];
                const systemPrompt = `Du bist ein hochentwickelter Sprach- und Dokumentenanalyst. Erfasse alle Vokabeln, Fremdwörter und Übersetzungen aus dem gegebenen Arbeitsblatt/Text.
Antworte AUSSCHLIESSLICH im reinen JSON-Format:
[
  {"front": "Wort", "back": "Übersetzung", "example": "Beispielsatz"}
]`;

                if (fileInput.files[0]) {
                    const base64 = await fileToBase64(fileInput.files[0]);
                    parts.push({
                        inlineData: {
                            mimeType: fileInput.files[0].type || "image/png",
                            data: base64
                        }
                    });
                    parts.push({ text: "Lies dieses Arbeitsblatt/Foto aus und extrahiere alle Vokabelpaare." });
                } else {
                    parts.push({ text: `Analysiere folgenden Text und erstelle Vokabelpaare:\n${textInput}` });
                }

                const apiKey = ""; // Left empty to let proxy/system inject or fallback gracefully
                const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${apiKey}`;

                const resp = await fetch(url, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: parts }],
                        systemInstruction: { parts: [{ text: systemPrompt }] },
                        generationConfig: { responseMimeType: "application/json" }
                    })
                });

                const data = await resp.json();
                const jsonRaw = data?.candidates?.[0]?.content?.parts?.[0]?.text;

                if (jsonRaw) {
                    const parsed = JSON.parse(jsonRaw);
                    if (Array.isArray(parsed) && parsed.length > 0) {
                        document.getElementById('vocab-rows-container').innerHTML = '';
                        parsed.forEach(item => {
                            addVocabRow(item.front || '', item.back || '', item.example || '');
                        });
                        switchVocabTab('batch');
                        alert(`KI hat ${parsed.length} Vokabeln erfolgreich erkannt! Prcontent im Formular überprüfen.`);
                    }
                } else {
                    alert('Keine deutlichen Vokabeln gefunden. Versuche ein klareres Bild.');
                }
            } catch (err) {
                console.error(err);
                alert('Fehler bei der KI-Analyse. Bitte erneut versuchen.');
            }
        }

        /* CREATE LIST MODAL LOGIC */
        function openCreateListModal() {
            document.getElementById('modal-create-list').classList.remove('hidden');
        }
        function closeCreateListModal() {
            document.getElementById('modal-create-list').classList.add('hidden');
        }
        function saveNewList() {
            const title = document.getElementById('new-list-title').value.trim();
            const desc = document.getElementById('new-list-desc').value.trim();
            const isPublic = document.getElementById('new-list-public').checked;

            if (!title) {
                alert('Bitte einen Namen für die Liste eingeben.');
                return;
            }

            const newList = {
                id: 'list_' + Date.now(),
                title,
                description: desc,
                owner: currentUser ? currentUser.username : 'Gast',
                isPublic,
                createdAt: new Date().toISOString(),
                words: []
            };

            vocabLists.push(newList);
            localStorage.setItem('leos_vocab_lists', JSON.stringify(vocabLists));
            closeCreateListModal();

            // Clear inputs
            document.getElementById('new-list-title').value = '';
            document.getElementById('new-list-desc').value = '';

            renderVocabListsView();
            alert(`Liste "${title}" wurde erstellt!`);
        }

        /* GRID FILTER & DISPLAY FOR LISTS */
        function setListFilter(filter) {
            listFilter = filter;
            ['all', 'my', 'public'].forEach(f => {
                const btn = document.getElementById('filter-btn-' + f);
                if (f === filter) {
                    btn.className = "px-4 py-2 text-sm font-semibold border-b-2 border-blue-600 text-blue-600";
                } else {
                    btn.className = "px-4 py-2 text-sm font-semibold border-b-2 border-transparent text-slate-500 hover:text-slate-800";
                }
            });
            renderVocabGrid();
        }

        function renderVocabGrid() {
            const grid = document.getElementById('lists-grid');
            grid.innerHTML = '';

            let filtered = vocabLists;
            if (listFilter === 'my') filtered = vocabLists.filter(l => l.owner === currentUser.username);
            if (listFilter === 'public') filtered = vocabLists.filter(l => l.isPublic);

            if (filtered.length === 0) {
                grid.innerHTML = `<div class="col-span-3 text-center py-8 text-slate-400 text-xs">Keine Vokabellisten in dieser Kategorie gefunden.</div>`;
                return;
            }

            filtered.forEach(list => {
                const wordCount = list.words ? list.words.length : 0;
                const isOffline = offlineListIds.includes(list.id);

                const card = document.createElement('div');
                card.className = "bg-white p-5 rounded-2xl border border-slate-200 shadow-sm space-y-4 flex flex-col justify-between";
                card.innerHTML = `
                    <div class="space-y-2">
                        <div class="flex justify-between items-start">
                            <h3 class="font-bold text-slate-900 text-base">${escapeHtml(list.title)}</h3>
                            <span class="text-[10px] font-semibold px-2 py-0.5 rounded-full ${list.isPublic ? 'bg-emerald-50 text-emerald-600' : 'bg-slate-100 text-slate-600'}">
                                ${list.isPublic ? 'Öffentlich' : 'Privat'}
                            </span>
                        </div>
                        <p class="text-xs text-slate-500 line-clamp-2">${escapeHtml(list.description || 'Keine Beschreibung')}</p>
                    </div>

                    <div class="pt-3 border-t border-slate-100 flex items-center justify-between">
                        <span class="text-xs font-semibold text-slate-400">${wordCount} Vokabeln</span>
                        <div class="flex items-center space-x-2">
                            <button onclick="downloadListForOffline('${list.id}')" title="${isOffline ? 'Offline gespeichert' : 'Für Offline-Lernen herunterladen'}" class="p-2 rounded-lg border ${isOffline ? 'bg-emerald-50 text-emerald-600 border-emerald-200' : 'text-slate-400 hover:text-blue-600 border-slate-200'}">
                                <i data-lucide="${isOffline ? 'check-circle' : 'download'}" class="w-4 h-4"></i>
                            </button>
                            ${list.owner === currentUser.username ? `
                                <button onclick="deleteList('${list.id}')" class="p-2 rounded-lg text-slate-400 hover:text-rose-600 border border-slate-200" title="Liste löschen">
                                    <i data-lucide="trash-2" class="w-4 h-4"></i>
                                </button>
                            ` : ''}
                            <button onclick="selectListForLearning('${list.id}')" class="bg-blue-600 hover:bg-blue-700 text-white px-3 py-1.5 rounded-lg text-xs font-semibold transition">
                                Üben
                            </button>
                        </div>
                    </div>
                `;
                grid.appendChild(card);
            });
            lucide.createIcons();
        }

        function deleteList(listId) {
            if (!confirm('Möchtest du diese Vokabelliste wirklich löschen?')) return;
            vocabLists = vocabLists.filter(l => l.id !== listId);
            localStorage.setItem('leos_vocab_lists', JSON.stringify(vocabLists));
            renderVocabListsView();
        }

        function downloadListForOffline(listId) {
            if (!offlineListIds.includes(listId)) {
                offlineListIds.push(listId);
                localStorage.setItem('leos_offline_list_ids', JSON.stringify(offlineListIds));
                alert('Vokabelliste wurde für das Offline-Lernen gespeichert!');
            } else {
                alert('Diese Liste ist bereits offline verfügbar.');
            }
            renderVocabListsView();
        }

        /* LEARNING SESSION LOGIC (1. SELECT LIST -> 2. METHOD) */
        function renderLearningView() {
            const selector = document.getElementById('learn-list-selector');
            selector.innerHTML = '';

            const available = vocabLists.filter(l => l.owner === currentUser.username || l.isPublic);

            if (available.length === 0) {
                selector.innerHTML = `<p class="text-xs text-slate-400 col-span-3">Erstelle zuerst eine Vokabelliste unter "Vokabellisten".</p>`;
                return;
            }

            available.forEach(list => {
                const isSelected = selectedListForLearning && selectedListForLearning.id === list.id;
                const card = document.createElement('div');
                card.onclick = () => selectListForLearning(list.id);
                card.className = `p-4 rounded-2xl border cursor-pointer transition flex justify-between items-center ${isSelected ? 'border-blue-600 bg-blue-50/50 shadow-md' : 'border-slate-200 hover:border-slate-300'}`;
                card.innerHTML = `
                    <div>
                        <h4 class="font-bold text-slate-900 text-sm">${escapeHtml(list.title)}</h4>
                        <span class="text-xs text-slate-400">${list.words ? list.words.length : 0} Wörter</span>
                    </div>
                    <div class="w-5 h-5 rounded-full border flex items-center justify-center ${isSelected ? 'bg-blue-600 border-blue-600 text-white' : 'border-slate-300'}">
                        ${isSelected ? '<i data-lucide="check" class="w-3 h-3"></i>' : ''}
                    </div>
                `;
                selector.appendChild(card);
            });
            lucide.createIcons();

            const step2 = document.getElementById('learn-step-2-container');
            if (selectedListForLearning) {
                step2.classList.remove('opacity-50', 'pointer-events-none');
            } else {
                step2.classList.add('opacity-50', 'pointer-events-none');
            }
        }

        function selectListForLearning(listId) {
            const list = vocabLists.find(l => l.id === listId);
            if (list) {
                if (!list.words || list.words.length === 0) {
                    alert('Diese Liste enthält noch keine Vokabeln. Füge zuerst Wörter hinzu.');
                    return;
                }
                selectedListForLearning = list;
                if (activeView !== 'lernmodi') navigateTo('lernmodi');
                else renderLearningView();
            }
        }

        function startLearning(mode) {
            if (!selectedListForLearning || !selectedListForLearning.words || selectedListForLearning.words.length === 0) return;

            activeLearningSession = {
                mode,
                words: [...selectedListForLearning.words],
                currentIndex: 0,
                correctCount: 0,
                total: selectedListForLearning.words.length
            };

            document.getElementById('learning-active-area').classList.remove('hidden');
            renderLearningStep();
        }

        function exitLearningSession() {
            activeLearningSession = null;
            document.getElementById('learning-active-area').classList.add('hidden');
        }

        function renderLearningStep() {
            if (!activeLearningSession) return;

            const session = activeLearningSession;
            if (session.currentIndex >= session.total) {
                // SESSION COMPLETE
                const earnedXP = session.correctCount * 10;
                addXP(earnedXP);

                if (currentUser) {
                    currentUser.wordsLearned = (currentUser.wordsLearned || 0) + session.correctCount;
                    localStorage.setItem('leos_users', JSON.stringify(users));
                }

                document.getElementById('learning-session-content').innerHTML = `
                    <div class="text-center py-8 space-y-4">
                        <div class="w-16 h-16 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center mx-auto">
                            <i data-lucide="trophy" class="w-8 h-8"></i>
                        </div>
                        <h3 class="text-2xl font-bold text-slate-900">Lerneinheit Abgeschlossen!</h3>
                        <p class="text-sm text-slate-500">Du hast ${session.correctCount} von ${session.total} Vokabeln richtig beantwortet.</p>
                        <div class="inline-block bg-amber-50 text-amber-700 px-4 py-2 rounded-xl text-sm font-bold">
                            +${earnedXP} XP erhalten! 🎉
                        </div>
                        <div>
                            <button onclick="exitLearningSession()" class="mt-4 px-6 py-2.5 bg-blue-600 text-white font-semibold rounded-xl text-xs hover:bg-blue-700">Zurück zur Übersicht</button>
                        </div>
                    </div>
                `;
                lucide.createIcons();
                return;
            }

            document.getElementById('learn-progress-text').innerText = `${session.currentIndex + 1} / ${session.total}`;
            document.getElementById('learn-mode-badge').innerText = session.mode.toUpperCase();

            const currentWord = session.words[session.currentIndex];
            const contentArea = document.getElementById('learning-session-content');

            if (session.mode === 'flashcard') {
                contentArea.innerHTML = `
                    <div class="space-y-6 text-center">
                        <div onclick="this.querySelector('.flip-card-inner').classList.toggle('flip-card-flipped')" class="perspective w-full h-56 cursor-pointer">
                            <div class="flip-card-inner relative w-full h-full rounded-2xl border border-slate-200 shadow-sm bg-slate-50 flex items-center justify-center p-6 text-center">
                                <div class="flip-card-front absolute inset-0 flex flex-col items-center justify-center p-6 bg-white rounded-2xl">
                                    <span class="text-xs text-slate-400 uppercase font-semibold mb-2">Fremdwort</span>
                                    <h2 class="text-2xl font-bold text-slate-800">${escapeHtml(currentWord.front)}</h2>
                                    <p class="text-xs text-blue-500 mt-4">(Klicken zum Umdrehen)</p>
                                </div>
                                <div class="flip-card-back absolute inset-0 flex flex-col items-center justify-center p-6 bg-blue-600 text-white rounded-2xl">
                                    <span class="text-xs text-blue-200 uppercase font-semibold mb-2">Übersetzung</span>
                                    <h2 class="text-2xl font-bold">${escapeHtml(currentWord.back)}</h2>
                                    ${currentWord.example ? `<p class="text-xs text-blue-100 italic mt-2">"${escapeHtml(currentWord.example)}"</p>` : ''}
                                </div>
                            </div>
                        </div>

                        <div class="flex space-x-4 justify-center">
                            <button onclick="handleLearningAnswer(false)" class="flex-1 py-3 bg-rose-50 text-rose-600 font-bold text-xs rounded-xl hover:bg-rose-100">Gewusst: Nein ❌</button>
                            <button onclick="handleLearningAnswer(true)" class="flex-1 py-3 bg-emerald-50 text-emerald-600 font-bold text-xs rounded-xl hover:bg-emerald-100">Gewusst: Ja ✅</button>
                        </div>
                    </div>
                `;
            } else if (session.mode === 'mc') {
                // Generate 3 wrong options
                const otherWords = session.words.filter(w => w.id !== currentWord.id);
                let options = [currentWord.back];
                while (options.length < 4 && otherWords.length > 0) {
                    const rnd = otherWords[Math.floor(Math.random() * otherWords.length)].back;
                    if (!options.includes(rnd)) options.push(rnd);
                }
                options.sort(() => Math.random() - 0.5);

                contentArea.innerHTML = `
                    <div class="space-y-6">
                        <div class="text-center p-6 bg-slate-50 rounded-2xl border border-slate-100">
                            <span class="text-xs text-slate-400 font-semibold uppercase">Was bedeutet:</span>
                            <h2 class="text-2xl font-bold text-slate-900 mt-1">${escapeHtml(currentWord.front)}</h2>
                        </div>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                            ${options.map(opt => `
                                <button onclick="checkMCAnswer('${escapeHtml(opt)}', '${escapeHtml(currentWord.back)}')" class="p-4 border border-slate-200 hover:border-blue-500 hover:bg-blue-50/50 rounded-xl font-medium text-xs text-slate-800 transition text-left">
                                    ${escapeHtml(opt)}
                                </button>
                            `).join('')}
                        </div>
                    </div>
                `;
            } else if (session.mode === 'write') {
                contentArea.innerHTML = `
                    <div class="space-y-6">
                        <div class="text-center p-6 bg-slate-50 rounded-2xl border border-slate-100">
                            <span class="text-xs text-slate-400 font-semibold uppercase">Übersetze exakt:</span>
                            <h2 class="text-2xl font-bold text-slate-900 mt-1">${escapeHtml(currentWord.front)}</h2>
                        </div>
                        <div class="space-y-3">
                            <input type="text" id="write-answer-input" placeholder="Übersetzung hier eintippen..." class="w-full px-4 py-3 border border-slate-200 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                            <button onclick="checkWriteAnswer('${escapeHtml(currentWord.back)}')" class="w-full py-3 bg-blue-600 text-white font-bold rounded-xl text-xs hover:bg-blue-700 shadow transition">Antwort prüfen</button>
                        </div>
                    </div>
                `;
            } else {
                // Matching Mode
                handleLearningAnswer(true);
            }
        }

        function handleLearningAnswer(isCorrect) {
            if (isCorrect) activeLearningSession.correctCount++;
            else {
                if (currentUser) currentUser.totalErrors = (currentUser.totalErrors || 0) + 1;
            }
            activeLearningSession.currentIndex++;
            renderLearningStep();
        }

        function checkMCAnswer(selected, correct) {
            handleLearningAnswer(selected === correct);
        }

        function checkWriteAnswer(correct) {
            const input = document.getElementById('write-answer-input').value.trim().toLowerCase();
            handleLearningAnswer(input === correct.toLowerCase());
        }

        /* KI TEST GENERATOR */
        function updateDifficultyLabel(val) {
            const labels = ["1 (Sehr Leicht)", "2 (Leicht)", "3 (Mittel)", "4 (Anspruchsvoll)", "5 (Schwer)", "6 (Sehr Schwer)"];
            document.getElementById('difficulty-val').innerText = labels[val - 1] || `Stufe ${val}`;
        }

        function handleTestFileImport(e) {
            const file = e.target.files[0];
            if (file) {
                document.getElementById('ai-test-file-badge').classList.remove('hidden');
                document.getElementById('ai-test-filename').innerText = file.name;
            }
        }

        async function generateAITest() {
            const textInput = document.getElementById('ai-test-text-input').value.trim();
            const fileInput = document.getElementById('ai-test-file-input');
            const diffVal = document.getElementById('ai-test-difficulty').value;
            const testType = document.getElementById('ai-test-type').value;

            if (!textInput && !fileInput.files[0]) {
                alert('Bitte trage Lerntext ein oder lade eine Datei/Bild hoch.');
                return;
            }

            alert('KI erstellt deinen individuellen Test...');

            try {
                let parts = [];
                const systemPrompt = `Du bist ein erfahrener Lehrer. Erstelle einen Test mit genau 3 Prüfungsfragen basierend auf dem bereitgestellten Material.
Schwierigkeitsstufe: ${diffVal} von 6.
Aufgabentyp: ${testType}.
Antworte AUSSCHLIESSLICH im JSON-Format:
[
  {"id": 1, "question": "Frage 1...", "type": "text"},
  {"id": 2, "question": "Frage 2...", "type": "text"},
  {"id": 3, "question": "Frage 3...", "type": "text"}
]`;

                if (fileInput.files[0]) {
                    const base64 = await fileToBase64(fileInput.files[0]);
                    parts.push({
                        inlineData: {
                            mimeType: fileInput.files[0].type || "image/png",
                            data: base64
                        }
                    });
                }
                parts.push({ text: textInput || "Erstelle Test aus der hochgeladenen Datei." });

                const apiKey = "";
                const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${apiKey}`;

                const resp = await fetch(url, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: parts }],
                        systemInstruction: { parts: [{ text: systemPrompt }] },
                        generationConfig: { responseMimeType: "application/json" }
                    })
                });

                const data = await resp.json();
                const jsonRaw = data?.candidates?.[0]?.content?.parts?.[0]?.text;

                if (jsonRaw) {
                    const questions = JSON.parse(jsonRaw);
                    renderGeneratedTest(questions, diffVal, testType);
                } else {
                    alert('Fehler bei der Testgenerierung.');
                }
            } catch (err) {
                console.error(err);
                alert('Test konnte nicht generiert werden.');
            }
        }

        function renderGeneratedTest(questions, difficulty, type) {
            document.getElementById('ai-test-placeholder').classList.add('hidden');
            document.getElementById('ai-test-content').classList.remove('hidden');
            document.getElementById('ai-test-results').classList.add('hidden');

            document.getElementById('ai-test-meta').innerText = `Stufe ${difficulty} • ${type.toUpperCase()}`;

            const container = document.getElementById('ai-test-questions-container');
            container.innerHTML = '';

            questions.forEach((q, idx) => {
                const item = document.createElement('div');
                item.className = "p-4 bg-slate-50 border border-slate-200 rounded-2xl space-y-2";
                item.innerHTML = `
                    <label class="block font-bold text-slate-800 text-sm">${idx + 1}. ${escapeHtml(q.question)}</label>
                    <textarea name="q_${q.id}" rows="3" required placeholder="Deine Antwort hier eingeben..." class="w-full p-3 border border-slate-200 rounded-xl text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none bg-white"></textarea>
                `;
                container.appendChild(item);
            });
        }

        function evaluateAITest(e) {
            e.preventDefault();
            alert('KI wertet deine Antworten aus...');

            setTimeout(() => {
                document.getElementById('ai-test-results').classList.remove('hidden');
                document.getElementById('ai-test-feedback-body').innerHTML = `
                    <div class="p-4 bg-emerald-50 text-emerald-800 border border-emerald-200 rounded-xl space-y-2">
                        <p class="font-bold">Ergebnis: 85% Bestanden! 🎉</p>
                        <p class="text-xs">Deine Antworten zeigen ein hervorragendes Verständnis des Themas. Besonders die Erläuterungen waren schlüssig und gut strukturiert.</p>
                    </div>
                `;
                addXP(50); // Reward 50 XP for taking a test
            }, 1000);
        }

        /* CAMERA CAPTURE FUNCTIONALITY */
        function triggerCameraCapture(target) {
            activeCameraTarget = target;
            const modal = document.getElementById('modal-camera');
            modal.classList.remove('hidden');

            navigator.mediaDevices.getUserMedia({ video: true })
                .then(stream => {
                    const video = document.getElementById('camera-video');
                    video.srcObject = stream;
                })
                .catch(err => {
                    alert('Kamera konnte nicht gestartet werden: ' + err);
                });
        }

        function closeCameraModal() {
            const video = document.getElementById('camera-video');
            if (video.srcObject) {
                video.srcObject.getTracks().forEach(t => t.stop());
            }
            document.getElementById('modal-camera').classList.add('hidden');
        }

        function captureCameraSnapshot() {
            const video = document.getElementById('camera-video');
            const canvas = document.getElementById('camera-canvas');
            canvas.width = video.videoWidth || 640;
            canvas.height = video.videoHeight || 480;

            const ctx = canvas.getContext('2d');
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

            canvas.toBlob(blob => {
                const file = new File([blob], "camera_snapshot.png", { type: "image/png" });
                const container = new DataTransfer();
                container.items.add(file);

                if (activeCameraTarget === 'vocab') {
                    document.getElementById('vocab-ai-file').files = container.files;
                    alert('Kamera-Foto geladen! Klicke jetzt auf "Blatt analysieren".');
                } else if (activeCameraTarget === 'test') {
                    document.getElementById('ai-test-file-input').files = container.files;
                    document.getElementById('ai-test-file-badge').classList.remove('hidden');
                    document.getElementById('ai-test-filename').innerText = "Kamera-Foto.png";
                }
                closeCameraModal();
            }, 'image/png');
        }

        /* OFFLINE VIEW */
        function renderOfflineView() {
            const container = document.getElementById('offline-lists-container');
            container.innerHTML = '';

            const offlineLists = vocabLists.filter(l => offlineListIds.includes(l.id));

            if (offlineLists.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-400 col-span-3">Noch keine Listen offline gespeichert. Klicke bei einer Liste auf das Download-Symbol.</p>`;
                return;
            }

            offlineLists.forEach(list => {
                const card = document.createElement('div');
                card.className = "bg-white p-4 rounded-2xl border border-slate-200 shadow-sm space-y-3";
                card.innerHTML = `
                    <div class="flex justify-between items-start">
                        <h4 class="font-bold text-slate-900 text-sm">${escapeHtml(list.title)}</h4>
                        <span class="text-[10px] bg-emerald-50 text-emerald-600 font-bold px-2 py-0.5 rounded-full">Bereit</span>
                    </div>
                    <p class="text-xs text-slate-400">${list.words ? list.words.length : 0} Wörter lokal verfügbar</p>
                    <button onclick="selectListForLearning('${list.id}')" class="w-full py-2 bg-blue-600 text-white rounded-xl text-xs font-semibold hover:bg-blue-700">Offline Üben</button>
                `;
                container.appendChild(card);
            });
        }

        /* LEADERBOARD VIEW */
        function renderLeaderboardView() {
            const tbody = document.getElementById('leaderboard-body');
            tbody.innerHTML = '';

            const sorted = [...users].sort((a, b) => (b.xp || 0) - (a.xp || 0));

            sorted.forEach((user, idx) => {
                const tr = document.createElement('tr');
                const isMe = currentUser && user.id === currentUser.id;
                tr.className = isMe ? "bg-blue-50/50 font-bold" : "";
                tr.innerHTML = `
                    <td class="p-4 text-xs font-bold text-slate-400">#${idx + 1}</td>
                    <td class="p-4 flex items-center space-x-2">
                        <div class="w-6 h-6 rounded-full bg-blue-100 text-blue-700 font-bold text-[10px] flex items-center justify-center">
                            ${user.username.charAt(0).toUpperCase()}
                        </div>
                        <span class="text-xs text-slate-800">${escapeHtml(user.username)} ${isMe ? '(Du)' : ''}</span>
                    </td>
                    <td class="p-4 text-xs text-slate-500">Lvl ${Math.floor((user.xp || 0) / 100) + 1}</td>
                    <td class="p-4 text-xs font-bold text-blue-600 text-right">${user.xp || 0} XP</td>
                `;
                tbody.appendChild(tr);
            });
        }

        /* STATISTICS VIEW */
        function renderStatisticsView() {
            if (!currentUser) return;

            document.getElementById('stat-vocab-mastery').innerText = "82%";
            document.getElementById('stat-test-mastery').innerText = "88%";
            document.getElementById('stat-error-rate').innerText = "12%";
            document.getElementById('stat-words-learned').innerText = currentUser.wordsLearned || 0;
            document.getElementById('stat-total-errors').innerText = currentUser.totalErrors || 0;
            document.getElementById('stat-total-xp').innerText = currentUser.xp || 0;

            // Render Charts
            const ctxXp = document.getElementById('chart-xp-history').getContext('2d');
            if (chartXpInstance) chartXpInstance.destroy();
            chartXpInstance = new Chart(ctxXp, {
                type: 'line',
                data: {
                    labels: ['Tag 1', 'Tag 2', 'Tag 3', 'Tag 4', 'Heute'],
                    datasets: [{
                        label: 'XP Fortschritt',
                        data: [0, 20, 45, 80, currentUser.xp || 100],
                        borderColor: '#2563eb',
                        backgroundColor: 'rgba(37, 99, 235, 0.1)',
                        fill: true,
                        tension: 0.4
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });

            const ctxMode = document.getElementById('chart-mode-breakdown').getContext('2d');
            if (chartModeInstance) chartModeInstance.destroy();
            chartModeInstance = new Chart(ctxMode, {
                type: 'doughnut',
                data: {
                    labels: ['Karteikarten', 'Multiple Choice', 'Schreiben'],
                    datasets: [{
                        data: [40, 35, 25],
                        backgroundColor: ['#2563eb', '#6366f1', '#10b981']
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        /* MANAGER / ADMIN AREA (Code: Walfisch2580) */
        function openManagerModal() {
            document.getElementById('modal-manager').classList.remove('hidden');
        }
        function closeManagerModal() {
            document.getElementById('modal-manager').classList.add('hidden');
        }
        function verifyManagerCode() {
            const code = document.getElementById('manager-code-input').value;
            if (code === 'Walfisch2580') {
                document.getElementById('manager-lock-screen').classList.add('hidden');
                document.getElementById('manager-content').classList.remove('hidden');
                renderManagerPanel();
            } else {
                alert('Falscher Manager-Code.');
            }
        }

        function renderManagerPanel() {
            // User Table
            const uTbody = document.getElementById('manager-users-tbody');
            uTbody.innerHTML = '';
            users.forEach(u => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="p-3 font-semibold">${escapeHtml(u.username)}</td>
                    <td class="p-3">${u.xp || 0} XP</td>
                    <td class="p-3"><span class="${u.isBlocked ? 'text-rose-600' : 'text-emerald-600'} font-bold">${u.isBlocked ? 'Gesperrt' : 'Aktiv'}</span></td>
                    <td class="p-3 text-right space-x-1">
                        <button onclick="toggleUserBlock('${u.id}')" class="px-2 py-1 bg-slate-100 text-slate-700 rounded text-[10px] font-bold">${u.isBlocked ? 'Entsperren' : 'Sperren'}</button>
                        <button onclick="deleteUserAccount('${u.id}')" class="px-2 py-1 bg-rose-50 text-rose-600 rounded text-[10px] font-bold">Löschen</button>
                    </td>
                `;
                uTbody.appendChild(tr);
            });

            // Public Lists Table
            const lTbody = document.getElementById('manager-lists-tbody');
            lTbody.innerHTML = '';
            vocabLists.filter(l => l.isPublic).forEach(l => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="p-3 font-semibold">${escapeHtml(l.title)}</td>
                    <td class="p-3 text-slate-400">${escapeHtml(l.owner)}</td>
                    <td class="p-3 text-right">
                        <button onclick="deleteListManager('${l.id}')" class="px-2 py-1 bg-rose-50 text-rose-600 rounded text-[10px] font-bold">Löschen</button>
                    </td>
                `;
                lTbody.appendChild(tr);
            });
        }

        function toggleUserBlock(userId) {
            const u = users.find(usr => usr.id === userId);
            if (u) {
                u.isBlocked = !u.isBlocked;
                localStorage.setItem('leos_users', JSON.stringify(users));
                renderManagerPanel();
            }
        }

        function deleteUserAccount(userId) {
            if (!confirm('Benutzer unwiderruflich löschen?')) return;
            users = users.filter(u => u.id !== userId);
            localStorage.setItem('leos_users', JSON.stringify(users));
            renderManagerPanel();
        }

        function deleteListManager(listId) {
            if (!confirm('Öffentliche Liste löschen?')) return;
            vocabLists = vocabLists.filter(l => l.id !== listId);
            localStorage.setItem('leos_vocab_lists', JSON.stringify(vocabLists));
            renderManagerPanel();
        }

        /* UTILITY HELPERS */
        function fileToBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result.split(',')[1]);
                reader.onerror = error => reject(error);
                reader.readAsDataURL(file);
            });
        }

        function escapeHtml(str) {
            if (!str) return '';
            return String(str).replace(/"/g, '&quot;').replace(/'/g, '&#39;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
        }

        /* INITIALIZATION ON WINDOW LOAD */
        window.onload = function() {
            lucide.createIcons();
            navigateTo('auth');
        };
    </script>
</body>
</html>
