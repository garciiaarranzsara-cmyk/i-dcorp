<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>I+D HUB - Sistema de Progresión</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        
        :root {
            --bg-dark: #020617;
            --card-bg: #0f172a;
            --accent: #4f46e5;
        }

        body { 
            font-family: 'Inter', sans-serif; 
            background-color: var(--bg-dark); 
            color: #e2e8f0; 
            margin: 0; 
        }

        .sidebar-active { 
            background-color: var(--accent); 
            color: white; 
            box-shadow: 0 10px 15px -3px rgba(79, 70, 229, 0.3); 
        }

        .fade-in { animation: fadeIn 0.4s ease-out forwards; }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .glass { 
            background: rgba(255, 255, 255, 0.03); 
            backdrop-filter: blur(10px); 
            border: 1px solid rgba(255, 255, 255, 0.05); 
        }

        .level-badge {
            background: linear-gradient(135deg, #6366f1 0%, #a855f7 100%);
        }

        .locked-card {
            filter: grayscale(1) opacity(0.5);
            cursor: not-allowed !important;
        }
    </style>
</head>
<body>

    <!-- Pantalla de Login -->
    <div id="login-screen" class="min-h-screen flex items-center justify-center p-4">
        <div class="max-w-md w-full bg-slate-900 border border-slate-800 rounded-[2.5rem] p-8 shadow-2xl text-center">
            <div class="bg-indigo-600/20 w-16 h-16 rounded-2xl flex items-center justify-center mx-auto mb-4 border border-indigo-500/30">
                <i data-lucide="lock" class="text-indigo-400 w-8 h-8"></i>
            </div>
            <h1 class="text-2xl font-black text-white italic uppercase tracking-tighter mb-2">Acceso I+D Corp</h1>
            <p class="text-slate-400 text-sm mb-8">Introduce tus credenciales de empleado</p>
            
            <form id="login-form" class="space-y-4 text-left">
                <div>
                    <label class="text-xs font-bold text-slate-500 uppercase ml-2 block mb-1">Email Corporativo</label>
                    <input type="email" id="email" value="Alex@idcorp.com" class="w-full bg-slate-800 border border-slate-700 rounded-2xl px-4 py-3 text-white">
                </div>
                <div>
                    <label class="text-xs font-bold text-slate-500 uppercase ml-2 block mb-1">Contraseña</label>
                    <input type="password" id="password" value="idcorp2024" class="w-full bg-slate-800 border border-slate-700 rounded-2xl px-4 py-3 text-white">
                </div>
                <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-black py-4 rounded-2xl shadow-lg flex items-center justify-center gap-2 uppercase tracking-widest text-sm mt-6">
                    Entrar <i data-lucide="chevron-right" class="w-5 h-5"></i>
                </button>
            </form>
        </div>
    </div>

    <!-- Pantalla Principal -->
    <div id="app-screen" class="min-h-screen hidden flex flex-col md:flex-row">
        <!-- Sidebar -->
        <aside class="w-full md:w-64 bg-slate-900 border-r border-slate-800 p-4 flex flex-col shrink-0">
            <div class="mb-8 p-2">
                <h2 class="text-indigo-400 font-black italic text-2xl tracking-tighter uppercase leading-none">I+D HUB</h2>
            </div>

            <!-- Nivel del Usuario -->
            <div class="mb-8 px-2">
                <div class="flex justify-between items-end mb-2">
                    <span class="text-[10px] font-black uppercase text-slate-500 tracking-widest text-left">Nivel Actual</span>
                    <span id="level-display" class="text-indigo-400 font-black italic">NIVEL 1</span>
                </div>
                <div class="w-full bg-slate-800 h-2 rounded-full overflow-hidden">
                    <div id="level-progress" class="level-badge h-full transition-all duration-500" style="width: 20%"></div>
                </div>
                <p id="xp-text" class="text-[9px] text-slate-500 mt-2 uppercase font-bold text-left">250 / 500 para Nivel 2</p>
            </div>

            <nav id="nav-links" class="flex-grow space-y-1">
                <button onclick="navigate('dashboard')" id="nav-dashboard" class="sidebar-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-bold text-xs transition-all sidebar-active">
                    <i data-lucide="layout-dashboard" class="w-4 h-4"></i> Dashboard
                </button>
                <button onclick="navigate('contrato')" id="nav-contrato" class="sidebar-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-bold text-xs transition-all text-slate-400 hover:bg-slate-800">
                    <i data-lucide="file-text" class="w-4 h-4"></i> Contrato 3D
                </button>
                <button onclick="navigate('misiones')" id="nav-misiones" class="sidebar-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-bold text-xs transition-all text-slate-400 hover:bg-slate-800">
                    <i data-lucide="zap" class="w-4 h-4"></i> Misiones Pro
                </button>
                <button onclick="navigate('micropolix')" id="nav-micropolix" class="sidebar-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-bold text-xs transition-all text-slate-400 hover:bg-slate-800">
                    <i data-lucide="map" class="w-4 h-4"></i> Micropolix
                </button>
                <button onclick="navigate('bonos')" id="nav-bonos" class="sidebar-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-bold text-xs transition-all text-slate-400 hover:bg-slate-800">
                    <i data-lucide="gift" class="w-4 h-4"></i> Bonificaciones
                </button>
            </nav>
            
            <div class="mt-auto p-2 pt-6 border-t border-slate-800">
                <div class="bg-amber-500/10 border border-amber-500/20 rounded-2xl p-4 flex items-center gap-3 mb-4 text-amber-500">
                    <i data-lucide="coins" class="w-5 h-5"></i>
                    <span id="bio-credits-display" class="font-black text-xl">250</span>
                    <span class="text-[10px] uppercase font-black opacity-60">Credits</span>
                </div>
                <button onclick="logout()" class="w-full flex items-center gap-3 px-4 py-2 text-rose-500 font-bold hover:bg-rose-500/10 rounded-xl transition-all text-xs uppercase tracking-widest">
                    <i data-lucide="log-out" class="w-4 h-4"></i> Salir
                </button>
            </div>
        </aside>

        <!-- Área de Contenido -->
        <main class="flex-grow p-6 md:p-12 overflow-y-auto" id="main-content">
            <!-- Inyección vía JS -->
        </main>
    </div>

    <script>
        // --- ESTADO ---
        let state = {
            coins: 250,
            totalEarned: 250, // Para calcular el nivel
            activeTab: 'dashboard',
            user: { name: "Alex I+D", dept: "Biotecnología" },
            doneDimensions: [false, false, false],
            currentMission: null,
            missionValidated: false
        };

        const LEVELS = [
            { lvl: 1, min: 0, title: 'Iniciador' },
            { lvl: 2, min: 500, title: 'Colaborador' },
            { lvl: 3, min: 1200, title: 'Conector' },
            { lvl: 4, min: 2500, title: 'Embajador' }
        ];

        // --- LÓGICA DE NIVELES ---
        function getCurrentLevel() {
            return [...LEVELS].reverse().find(l => state.totalEarned >= l.min) || LEVELS[0];
        }

        function getNextLevel() {
            const currentIdx = LEVELS.findIndex(l => l.lvl === getCurrentLevel().lvl);
            return LEVELS[currentIdx + 1] || null;
        }

        function updateUI() {
            const current = getCurrentLevel();
            const next = getNextLevel();
            
            document.getElementById('bio-credits-display').innerText = state.coins;
            document.getElementById('level-display').innerText = `NIVEL ${current.lvl}: ${current.title}`;
            
            const progressDisplay = document.getElementById('level-progress');
            const xpText = document.getElementById('xp-text');

            if (next) {
                const range = next.min - current.min;
                const progress = state.totalEarned - current.min;
                const pct = Math.min((progress / range) * 100, 100);
                progressDisplay.style.width = pct + '%';
                xpText.innerText = `${state.totalEarned} / ${next.min} PARA NIVEL ${next.lvl}`;
            } else {
                progressDisplay.style.width = '100%';
                xpText.innerText = "NIVEL MÁXIMO ALCANZADO";
            }
        }

        // --- NAVEGACIÓN ---
        function navigate(tab) {
            state.activeTab = tab;
            updateSidebar();
            renderContent();
        }

        function updateSidebar() {
            document.querySelectorAll('.sidebar-btn').forEach(btn => {
                btn.classList.remove('sidebar-active');
                btn.classList.add('text-slate-400');
            });
            const activeBtn = document.getElementById('nav-' + state.activeTab);
            if(activeBtn) {
                activeBtn.classList.add('sidebar-active');
                activeBtn.classList.remove('text-slate-400');
            }
        }

        function renderContent() {
            const container = document.getElementById('main-content');
            container.innerHTML = `
                <header class="mb-10 fade-in text-left">
                    <h1 class="text-3xl md:text-4xl font-black text-white italic uppercase tracking-tighter leading-none mb-2">
                        ${getTitle(state.activeTab)}
                    </h1>
                    <p class="text-slate-500 font-bold uppercase text-[10px] tracking-widest">
                        ${state.user.name} • ${state.user.dept}
                    </p>
                </header>
                <div class="fade-in">
                    ${getViewHTML(state.activeTab)}
                </div>
            `;
            lucide.createIcons();
            updateUI();
        }

        function getTitle(tab) {
            return {
                dashboard: 'Ecosistema de Impacto',
                contrato: 'Dimensión 1: Contrato 3D',
                misiones: 'Dimensión 2: Misiones Pro',
                micropolix: 'Dimensión 3: Micropolix',
                bonos: 'Marketplace de Premios'
            }[tab] || '';
        }

        function getViewHTML(tab) {
            switch(tab) {
                case 'dashboard':
                    return `
                        <div class="grid lg:grid-cols-3 gap-8 text-left">
                            <div class="lg:col-span-2 space-y-6">
                                <div class="bg-gradient-to-br from-indigo-600 to-indigo-900 rounded-[2.5rem] p-8 md:p-10 shadow-2xl relative overflow-hidden text-white">
                                    <div class="relative z-10">
                                        <h2 class="text-3xl font-black italic uppercase leading-tight mb-2">Revolución Cultural</h2>
                                        <p class="text-indigo-100/80 max-w-sm text-sm">Escanea QRs con tus compañeros para ganar Bio-Credits y subir de nivel.</p>
                                        <button onclick="navigate('misiones')" class="mt-6 bg-white text-indigo-900 px-6 py-2 rounded-xl font-black text-[10px] uppercase tracking-widest">Nueva Misión</button>
                                    </div>
                                    <i data-lucide="sparkles" class="absolute right-[-20px] bottom-[-20px] opacity-10 w-48 h-48 rotate-12"></i>
                                </div>
                                <div class="grid grid-cols-2 gap-4">
                                    <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6">
                                        <p class="text-slate-500 font-bold uppercase text-[9px] mb-2 tracking-widest">Exp. Total</p>
                                        <p class="text-3xl font-black text-white leading-none">${state.totalEarned}</p>
                                    </div>
                                    <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 text-indigo-400">
                                        <p class="text-slate-500 font-bold uppercase text-[9px] mb-2 tracking-widest">Rango</p>
                                        <p class="text-xl font-black uppercase italic">${getCurrentLevel().title}</p>
                                    </div>
                                </div>
                            </div>
                            <div class="bg-slate-900 border border-slate-800 rounded-[2.5rem] p-6">
                                <h3 class="text-sm font-black text-white italic uppercase mb-6 flex items-center gap-2">
                                    <i data-lucide="activity" class="text-indigo-400 w-4 h-4"></i> Actividad
                                </h3>
                                <div class="space-y-4">
                                    <div class="p-3 bg-white/5 rounded-xl border border-white/5">
                                        <p class="text-[10px] font-bold text-indigo-300 uppercase mb-1">Misiones Pro</p>
                                        <p class="text-[10px] text-slate-400 leading-relaxed">Completa misiones con un QR para ganar +25 puntos.</p>
                                    </div>
                                    <div class="p-3 border border-white/5 rounded-xl opacity-50">
                                        <p class="text-[10px] font-bold text-slate-500 uppercase mb-1">Contrato 3D</p>
                                        <p class="text-[10px] text-slate-500">Define tus 3 visiones para ganar +50 puntos.</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    `;
                case 'contrato':
                    return `
                        <div class="space-y-8 text-left">
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                                ${renderDimCard(0, 'user', 'Cómo me veo', 'indigo', 'Propósito personal.')}
                                ${renderDimCard(1, 'users', 'Cómo me ven', 'purple', 'Impacto en el equipo.')}
                                ${renderDimCard(2, 'trending-up', 'Desarrollo', 'amber', 'Plan a 90 días.')}
                            </div>
                            <div class="bg-slate-900 border border-slate-800 rounded-3xl p-8 text-[11px] text-slate-400">
                                <h4 class="font-black text-white uppercase italic mb-2">Las 3 Dimensiones del Talento</h4>
                                En I+D Corp, tú eres el centro. El Contrato 3D asegura que tu visión propia y la de tu equipo coincidan con tu crecimiento. Cada dimensión te otorga 50 Bio-Credits.
                            </div>
                        </div>
                    `;
                case 'misiones':
                    return `
                        <div class="max-w-xl mx-auto bg-slate-900 border border-slate-800 rounded-[3rem] p-10 text-center">
                            <div class="bg-indigo-600/20 w-16 h-16 rounded-2xl flex items-center justify-center mx-auto mb-6 border border-indigo-500/20">
                                <i data-lucide="zap" class="text-indigo-400 w-8 h-8"></i>
                            </div>
                            <h2 class="text-2xl font-black text-white italic uppercase mb-2">Misiones Flash Pro</h2>
                            <p class="text-slate-400 text-xs mb-8">Conecta con alguien de otro departamento para validar tu impacto.</p>
                            
                            ${state.currentMission ? `
                                <div class="bg-slate-800 border border-slate-700 rounded-[2rem] p-8 mb-6 animate-in zoom-in">
                                    <p class="text-xs font-black text-indigo-400 uppercase tracking-widest mb-4">Misión Asignada</p>
                                    <p class="text-lg text-white font-bold mb-6 leading-tight">
                                        Busca a <span class="text-indigo-300 font-black">${state.currentMission.n}</span><br>
                                        y descubre su <span class="italic text-indigo-300 font-black">"${state.currentMission.t}"</span>
                                    </p>
                                    
                                    <!-- QR CODE SECTION -->
                                    <div class="bg-white p-4 rounded-xl inline-block mb-6 shadow-xl">
                                        <img src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=VALIDATE-MISSION-${Date.now()}" 
                                             alt="QR Code" class="w-32 h-32" />
                                    </div>
                                    <p class="text-[10px] text-slate-500 uppercase font-bold mb-6">Tu compañero debe escanear esto para validar</p>
                                    
                                    <button onclick="completeMission()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-8 py-3 rounded-xl font-black text-[10px] uppercase tracking-widest">
                                        Simular Escaneo (Validar)
                                    </button>
                                </div>
                            ` : `
                                <button onclick="generateMission()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-10 py-4 rounded-2xl font-black text-xs uppercase shadow-lg">Solicitar Misión +25 XP</button>
                            `}
                        </div>
                    `;
                case 'micropolix':
                    return `
                        <div class="max-w-2xl mx-auto bg-slate-900 border border-slate-800 rounded-[3rem] p-10 text-left">
                            <div class="flex items-center gap-4 mb-8">
                                <i data-lucide="building-2" class="text-amber-500 w-8 h-8"></i>
                                <h2 class="text-xl font-black text-white italic uppercase tracking-tighter">Micropolix: Inmersión Técnica</h2>
                            </div>
                            <form id="micro-form" onsubmit="handleMicro(event)" class="space-y-6">
                                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                    <select class="bg-slate-800 border border-slate-700 rounded-xl px-4 py-3 text-xs text-white outline-none">
                                        <option>Ingeniería de Procesos</option>
                                        <option>Análisis Químico</option>
                                        <option>Desarrollo I+D</option>
                                    </select>
                                    <input type="text" placeholder="Compañero referente" class="bg-slate-800 border border-slate-700 rounded-xl px-4 py-3 text-xs text-white outline-none" required>
                                </div>
                                <textarea class="w-full bg-slate-800 border border-slate-700 rounded-xl px-4 py-3 text-xs text-white outline-none" rows="3" placeholder="¿Qué quieres observar hoy?"></textarea>
                                <button type="submit" class="w-full bg-amber-500 text-slate-900 font-black py-4 rounded-xl uppercase text-[10px] tracking-widest shadow-lg hover:bg-amber-400">Solicitar Visita Semestral</button>
                            </form>
                            <div id="micro-success" class="hidden text-center py-6">
                                <i data-lucide="check-circle" class="text-emerald-500 w-12 h-12 mx-auto mb-2"></i>
                                <p class="text-xs font-bold uppercase">Solicitud registrada</p>
                            </div>
                        </div>
                    `;
                case 'bonos':
                    return `
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 text-center">
                            ${renderBono('50€ Ticket Restaurante', 150, 'utensils', 1)}
                            ${renderBono('Café Gourmet Semanal', 50, 'coffee', 1)}
                            ${renderBono('Tarde de Viernes Libre', 600, 'clock', 2)}
                            ${renderBono('Formación Avanzada', 400, 'graduation-cap', 2)}
                            ${renderBono('Día Cumpleaños Extra', 1000, 'cake', 3)}
                            ${renderBono('Pack Merchandising Pro', 800, 'package', 3)}
                        </div>
                    `;
            }
        }

        // --- COMPONENTES DE UI ---
        function renderDimCard(idx, icon, title, color, desc) {
            const isDone = state.doneDimensions[idx];
            return `
                <div class="p-6 rounded-[2rem] border ${isDone ? 'bg-emerald-500/5 border-emerald-500/20' : 'bg-slate-900 border-slate-800'}">
                    <i data-lucide="${icon}" class="text-${color}-400 mb-4 w-8 h-8"></i>
                    <h3 class="text-sm font-black text-white uppercase italic mb-2">${title}</h3>
                    <p class="text-slate-400 text-[10px] leading-relaxed mb-6">${desc}</p>
                    <button onclick="completeDimension(${idx})" ${isDone ? 'disabled' : ''} 
                        class="w-full py-2 rounded-xl font-black text-[9px] uppercase tracking-widest ${isDone ? 'bg-emerald-500 text-white' : 'bg-indigo-600 text-white shadow-lg'}">
                        ${isDone ? 'COMPLETADO +50' : 'RELLENAR'}
                    </button>
                </div>
            `;
        }

        function renderBono(name, cost, icon, reqLvl) {
            const currentLvl = getCurrentLevel().lvl;
            const isLocked = currentLvl < reqLvl;
            const canAfford = state.coins >= cost;
            
            return `
                <div class="bg-slate-900 border border-slate-800 rounded-[2rem] p-6 flex flex-col h-full items-center ${isLocked ? 'locked-card' : ''}">
                    <div class="text-[10px] font-black ${isLocked ? 'text-slate-500' : 'text-indigo-400'} uppercase mb-4 tracking-widest">
                        REQUERIDO: LVL ${reqLvl}
                    </div>
                    <div class="bg-indigo-500/10 w-12 h-12 rounded-2xl flex items-center justify-center mb-4 text-indigo-400 border border-indigo-500/20">
                        <i data-lucide="${icon}" class="w-6 h-6"></i>
                    </div>
                    <h4 class="text-white font-black uppercase italic text-[11px] mb-4 leading-tight flex-grow">${name}</h4>
                    <p class="text-amber-500 font-black text-[10px] mb-6 flex items-center gap-1">
                        <i data-lucide="coins" class="w-3 h-3"></i> ${cost} CREDITS
                    </p>
                    <button onclick="buyBono(${cost})" ${isLocked || !canAfford ? 'disabled' : ''} 
                        class="w-full py-2 rounded-xl font-black text-[9px] uppercase ${!isLocked && canAfford ? 'bg-indigo-600 text-white shadow-lg shadow-indigo-600/20' : 'bg-slate-800 text-slate-600'}">
                        ${isLocked ? 'NIVEL BLOQUEADO' : (canAfford ? 'CANJEAR' : 'FALTAN CRÉDITOS')}
                    </button>
                </div>
            `;
        }

        // --- ACCIONES ---
        function completeDimension(idx) {
            state.doneDimensions[idx] = true;
            addRewards(50);
            renderContent();
        }

        function generateMission() {
            const names = ["Elena (Legal)", "Marcos (IT)", "Sofía (RRHH)", "Carlos (CEO)"];
            const tasks = ["libro favorito", "destino soñado", "mayor miedo", "superpoder deseado"];
            state.currentMission = {
                n: names[Math.floor(Math.random() * names.length)],
                t: tasks[Math.floor(Math.random() * tasks.length)]
            };
            renderContent();
        }

        function completeMission() {
            state.currentMission = null;
            addRewards(25);
            renderContent();
        }

        function buyBono(cost) {
            if(state.coins >= cost) {
                state.coins -= cost;
                renderContent();
            }
        }

        function handleMicro(e) {
            e.preventDefault();
            document.getElementById('micro-form').classList.add('hidden');
            document.getElementById('micro-success').classList.remove('hidden');
            lucide.createIcons();
        }

        function addRewards(amount) {
            state.coins += amount;
            state.totalEarned += amount;
            updateUI();
        }

        function logout() {
            state.isAuthenticated = false;
            document.getElementById('app-screen').classList.add('hidden');
            document.getElementById('login-screen').classList.remove('hidden');
        }

        // --- LOGIN ---
        document.getElementById('login-form').onsubmit = (e) => {
            e.preventDefault();
            if(document.getElementById('email').value.toLowerCase() === 'alex@idcorp.com' && document.getElementById('password').value === 'idcorp2024') {
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-screen').classList.remove('hidden');
                renderContent();
            }
        };

        window.onload = () => lucide.createIcons();
    </script>
</body>
</html>
