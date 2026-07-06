# Hub-key
Crie sua Key compartilhe ou coloque sua Key em aplicativos
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>H4X Key System v3.1</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.8/dist/chart.umd.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.1/build/qrcode.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    primaryColor: '#7C3AED',
                    colors: {
                        primary: { DEFAULT: '#7C3AED', 50: '#F5F3FF', 100: '#EDE9FE', 200: '#DDD6FE', 300: '#C4B5FD', 400: '#A78BFA', 500: '#8B5CF6', 600: '#7C3AED', 700: '#6D28D9', 800: '#5B21B6', 900: '#4C1D95' },
                        secondary: '#1E1B4B',
                        accent: '#06B6D4',
                        success: '#10B981',
                        warning: '#F59E0B',
                        danger: '#EF4444',
                        dark: '#0F172A',
                        light: '#F8FAFC'
                    },
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    animation: {
                        'pulse-slow': 'pulse 4s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                        'float': 'float 3s ease-in-out infinite',
                        'glow': 'glow 2s ease-in-out infinite alternate',
                        'typing': 'typing 2s steps(20, end) forwards',
                        'blink': 'blink 1s step-end infinite',
                        'bounce-in': 'bounceIn 0.5s ease forwards'
                    },
                    keyframes: {
                        float: { '0%,100%': { transform: 'translateY(0)' }, '50%': { transform: 'translateY(-8px)' } },
                        glow: { '0%': { boxShadow: '0 0 5px #7C3AED, 0 0 10px #7C3AED' }, '100%': { boxShadow: '0 0 15px #7C3AED, 0 0 30px #7C3AED' } },
                        typing: { 'from': { width: 0 }, 'to': { width: '100%' } },
                        blink: { '50%': { borderColor: 'transparent' } },
                        bounceIn: { '0%': { transform: scale(0.8); opacity: 0 }, '70%': { transform: scale(1.05) }, '100%': { transform: scale(1); opacity: 1 } }
                    }
                }
            }
        }
    </script>
    <style>
        * { cursor: url('https://cdn-icons-png.flaticon.com/32/109/109616.png'), auto !important; }
        ::selection { background: #7C3AED; color: white; }
        
        #particles { position: fixed; inset: 0; z-index: -2; background: #0F172A; }
        .dark #particles { background: #F8FAFC; }
        
        .glass {
            background: rgba(15, 23, 42, 0.7);
            backdrop-filter: blur(16px) saturate(180%);
            border: 1px solid rgba(124, 58, 237, 0.18);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
        }
        .dark .glass { background: rgba(255, 255, 255, 0.7); border-color: rgba(124, 58, 237, 0.2); }
        
        .neon-btn { position: relative; overflow: hidden; transition: all 0.3s ease; }
        .neon-btn::before {
            content: ''; position: absolute; inset: -2px; border-radius: inherit;
            background: linear-gradient(45deg, #7C3AED, #06B6D4, #10B981, #7C3AED);
            background-size: 400%; z-index: -1; filter: blur(5px);
            animation: glow 8s linear infinite; opacity: 0; transition: opacity 0.3s;
        }
        .neon-btn:hover::before { opacity: 1; }
        .neon-btn:active { transform: scale(0.98); }
        
        .strength-bar { height: 4px; border-radius: 2px; transition: all 0.3s; }
        .typing-effect { overflow: hidden; white-space: nowrap; border-right: 2px solid #7C3AED; animation: typing 2s steps(25) forwards, blink 0.7s infinite; }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: rgba(124, 58, 237, 0.1); border-radius: 3px; }
        ::-webkit-scrollbar-thumb { background: #7C3AED; border-radius: 3px; }
        .loading-bar { transition: width 0.3s ease; }
        .friend-card { transition: all 0.3s ease; }
        .friend-card:hover { transform: translateY(-3px); }
    </style>
</head>
<body class="bg-dark text-gray-100 dark:bg-light dark:text-gray-900 font-sans overflow-x-hidden">
    <!-- Fundo de Partículas -->
    <canvas id="particles"></canvas>

    <!-- Tela de Carregamento -->
    <div id="loadingScreen" class="fixed inset-0 z-[100] flex flex-col items-center justify-center bg-dark dark:bg-light transition-opacity duration-500">
        <div class="text-center mb-8">
            <i class="fa fa-key text-6xl text-primary animate-float mb-4"></i>
            <h1 class="text-2xl font-bold mb-2">H4X SYSTEM</h1>
            <p class="text-gray-400 dark:text-gray-600">Carregando componentes...</p>
        </div>
        <div class="w-64 h-2 bg-gray-700 dark:bg-gray-200 rounded-full overflow-hidden">
            <div id="loadingProgress" class="loading-bar h-full bg-primary rounded-full" style="width: 0%"></div>
        </div>
        <p id="loadingPercent" class="text-sm mt-2 text-gray-400 dark:text-gray-600">0%</p>
    </div>

    <!-- Menu Lateral -->
    <div id="sidebar" class="fixed top-0 left-0 h-full w-0 glass z-50 overflow-hidden transition-all duration-300">
        <div class="p-4 w-64">
            <div class="flex justify-between items-center mb-6">
                <h3 class="font-bold text-lg">Menu</h3>
                <button id="closeSidebar" class="p-2 rounded-lg hover:bg-gray-700/50 dark:hover:bg-gray-200/50">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <nav class="space-y-2">
                <button class="nav-btn w-full text-left px-4 py-3 rounded-lg hover:bg-primary/10 flex items-center gap-3" data-page="dashboard">
                    <i class="fa fa-home w-5"></i> Painel
                </button>
                <button class="nav-btn w-full text-left px-4 py-3 rounded-lg hover:bg-primary/10 flex items-center gap-3" data-page="keys">
                    <i class="fa fa-key w-5"></i> Minhas Chaves
                </button>
                <button class="nav-btn w-full text-left px-4 py-3 rounded-lg hover:bg-primary/10 flex items-center gap-3" data-page="friends">
                    <i class="fa fa-users w-5"></i> Amigos
                </button>
                <button class="nav-btn w-full text-left px-4 py-3 rounded-lg hover:bg-primary/10 flex items-center gap-3" data-page="history">
                    <i class="fa fa-history w-5"></i> Histórico
                </button>
                <button class="nav-btn w-full text-left px-4 py-3 rounded-lg hover:bg-primary/10 flex items-center gap-3" data-page="settings">
                    <i class="fa fa-cog w-5"></i> Configurações
                </button>
                <button class="nav-btn w-full text-left px-4 py-3 rounded-lg hover:bg-primary/10 flex items-center gap-3 text-danger" id="logoutBtn">
                    <i class="fa fa-sign-out w-5"></i> Sair
                </button>
            </nav>
            <div class="absolute bottom-4 left-4 right-4 text-center text-xs text-gray-500">
                <p>Versão 3.1.0</p>
                <p>© 2026 H4X System</p>
            </div>
        </div>
    </div>
    <div id="sidebarOverlay" class="fixed inset-0 bg-black/50 z-40 hidden transition-opacity duration-300"></div>

    <!-- Tela de Login -->
    <div id="loginScreen" class="fixed inset-0 z-30 flex items-center justify-center p-4 hidden">
        <div class="glass rounded-2xl p-6 w-full max-w-md">
            <div class="text-center mb-6">
                <div class="w-20 h-20 rounded-full bg-primary/20 flex items-center justify-center mx-auto mb-4">
                    <i class="fa fa-user-circle text-4xl text-primary"></i>
                </div>
                <h1 class="text-2xl font-bold">Acesse o Sistema</h1>
                <p class="text-sm text-gray-400 dark:text-gray-600">Entre com suas credenciais</p>
            </div>

            <form id="loginForm" class="space-y-4">
                <div class="space-y-2">
                    <label class="text-sm font-medium">Nome de Usuário</label>
                    <input type="text" id="loginUser" placeholder="Digite seu nome" class="w-full px-4 py-3 rounded-lg bg-secondary/30 dark:bg-gray-100 border border-primary/30 outline-none focus:ring-2 focus:ring-primary/50" maxlength="20">
                </div>

                <div class="space-y-2">
                    <div class="flex justify-between items-center">
                        <label class="text-sm font-medium">Senha</label>
                        <button type="button" id="togglePass" class="text-sm text-primary hover:underline">
                            <i class="fa fa-eye-slash"></i> Ocultar
                        </button>
                    </div>
                    <div class="relative">
                        <input type="password" id="loginPass" placeholder="Digite sua senha" class="w-full px-4 py-3 rounded-lg bg-secondary/30 dark:bg-gray-100 border border-primary/30 outline-none focus:ring-2 focus:ring-primary/50 pr-12" minlength="6">
                    </div>
                    <div class="strength-bar w-full bg-gray-700 dark:bg-gray-200 rounded-full overflow-hidden">
                        <div id="passStrength" class="strength-bar w-0 bg-danger"></div>
                    </div>
                    <p id="strengthText" class="text-xs text-gray-400">Força da senha</p>
                </div>

                <label class="flex items-center gap-2 cursor-pointer">
                    <input type="checkbox" id="rememberMe" class="w-4 h-4 rounded text-primary">
                    <span class="text-sm">Lembrar de mim</span>
                </label>

                <button type="submit" class="neon-btn w-full py-3 rounded-lg bg-primary text-white font-semibold">
                    <i class="fa fa-unlock-alt mr-2"></i> Entrar / Cadastrar
                </button>
            </form>
        </div>
    </div>

    <!-- Conteúdo Principal -->
    <div id="mainContent" class="relative z-10 p-4 max-w-4xl mx-auto pb-20 hidden">
        <!-- Cabeçalho -->
        <header class="glass rounded-xl p-4 mb-6 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <button id="openSidebar" class="p-2 rounded-lg hover:bg-primary/10">
                    <i class="fa fa-bars text-xl"></i>
                </button>
                <div>
                    <h2 class="font-bold">Olá, <span id="userName">Usuário</span></h2>
                    <p id="currentDateTime" class="text-xs text-gray-400 dark:text-gray-600"></p>
                </div>
            </div>
            <div class="flex items-center gap-2">
                <button id="themeToggle" class="p-2 rounded-lg hover:bg-primary/10" title="Alternar Tema">
                    <i class="fa fa-moon-o dark:hidden"></i>
                    <i class="fa fa-sun-o hidden dark:inline"></i>
                </button>
                <button id="supportBtn" class="p-2 rounded-lg hover:bg-primary/10" title="Suporte">
                    <i class="fa fa-headphones"></i>
                </button>
            </div>
        </header>

        <!-- Página: Painel -->
        <div id="dashboardPage" class="page">
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
                <div class="glass rounded-xl p-4">
                    <p class="text-xs text-gray-400 dark:text-gray-600">Nível</p>
                    <h3 class="text-2xl font-bold">12</h3>
                    <p class="text-xs text-success">+2 esse mês</p>
                </div>
                <div class="glass rounded-xl p-4">
                    <p class="text-xs text-gray-400 dark:text-gray-600">Chaves Geradas</p>
                    <h3 class="text-2xl font-bold" id="totalKeys">0</h3>
                    <p class="text-xs text-primary">Total</p>
                </div>
                <div class="glass rounded-xl p-4">
                    <p class="text-xs text-gray-400 dark:text-gray-600">Amigos</p>
                    <h3 class="text-2xl font-bold" id="totalFriends">0</h3>
                    <p class="text-xs text-accent">Conectados</p>
                </div>
                <div class="glass rounded-xl p-4">
                    <p class="text-xs text-gray-400 dark:text-gray-600">Último Login</p>
                    <h3 class="text-sm font-bold" id="lastLogin">-</h3>
                    <p class="text-xs text-gray-400 dark:text-gray-600">Data</p>
                </div>
            </div>

            <div class="glass rounded-xl p-6 mb-6">
                <h3 class="text-xl font-bold mb-4">Status da Sua Chave</h3>
                <div class="flex justify-between items-center mb-2">
                    <span class="text-sm">Validade</span>
                    <span id="keyStatus" class="text-sm font-bold text-warning">Nenhuma chave ativa</span>
                </div>
                <div class="w-full h-3 bg-gray-700 dark:bg-gray-200 rounded-full overflow-hidden mb-4">
                    <div id="validityProgress" class="h-full bg-primary rounded-full" style="width: 0%"></div>
                </div>
                <p id="timeRemaining" class="text-sm text-gray-400 dark:text-gray-600">Gere uma chave para ver o tempo restante</p>
            </div>
        </div>

        <!-- Página: Chaves -->
        <div id="keysPage" class="page hidden">
            <div class="glass rounded-xl p-6 mb-6">
                <h3 class="text-xl font-bold mb-4">Gerar Nova Chave</h3>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-3 mb-4">
                    <button class="duration-btn glass rounded-lg p-4 text-center hover:border-primary border-2 border-transparent transition" data-value="1h">
                        <i class="fa fa-clock-o text-2xl mb-2 text-warning"></i>
                        <p class="font-bold">1 Hora</p>
                    </button>
                    <button class="duration-btn glass rounded-lg p-4 text-center hover:border-primary border-2 border-transparent transition" data-value="1d">
                        <i class="fa fa-calendar-day text-2xl mb-2 text-accent"></i>
                        <p class="font-bold">1 Dia</p>
                    </button>
                    <button class="duration-btn glass rounded-lg p-4 text-center hover:border-primary border-2 border-transparent transition" data-value="perm">
                        <i class="fa fa-infinity text-2xl mb-2 text-success"></i>
                        <p class="font-bold">Permanente</p>
                    </button>
                </div>
                <button id="generateKey" class="neon-btn w-full py-3 rounded-lg bg-primary text-white font-semibold mb-4">
                    <i class="fa fa-magic mr-2"></i> Gerar Chave
                </button>

                <div id="keyResult" class="hidden mt-6">
                    <div class="bg-secondary/30 dark:bg-gray-100 rounded-lg p-4 mb-4">
                        <p class="text-sm text-gray-400 dark:text-gray-600 mb-2">Sua chave:</p>
                        <p id="generatedKey" class="typing-effect font-mono text-lg font-bold text-primary"></p>
                    </div>
                    <div class="grid grid-cols-2 md:grid-cols-4 gap-2 mb-4">
                        <button id="copyKey" class="py-2 rounded-lg bg-success text-white hover:bg-success/90 transition text-sm">
                            <i class="fa fa-copy mr-1"></i> Copiar
                        </button>
                        <button id="shareKey" class="py-2 rounded-lg bg-accent text-white hover:bg-accent/90 transition text-sm">
                            <i class="fa fa-share-alt mr-1"></i> Compartilhar
                        </button>
                        <button id="exportKey" class="py-2 rounded-lg bg-warning text-white hover:bg-warning/90 transition text-sm">
                            <i class="fa fa-download mr-1"></i> Exportar
                        </button>
                        <button id="showQR" class="py-2 rounded-lg bg-purple-500 text-white hover:bg-purple-500/90 transition text-sm">
                            <i class="fa fa-qrcode mr-1"></i> QR Code
                        </button>
                    </div>
                    <p class="text-xs mt-3 text-gray-400 dark:text-gray-600">Copiada <span id="copyCount">0</span> vezes</p>
                </div>
            </div>
        </div>

        <!-- Página: Amigos -->
        <div id="friendsPage" class="page hidden">
            <div class="glass rounded-xl p-6 mb-6">
                <h3 class="text-xl font-bold mb-4">Sistema de Amigos</h3>
                
                <div class="mb-6">
                    <div class="flex gap-2">
                        <input type="text" id="searchUser" placeholder="Pesquisar usuário..." class="flex-1 px-4 py-3 rounded-lg bg-secondary/30 dark:bg-gray-100 border border-primary/30 outline-none">
                        <button id="searchBtn" class="px-4 py-3 rounded-lg bg-primary text-white hover:bg-primary/90">
                            <i class="fa fa-search"></i>
                        </button>
                    </div>
                    <div id="searchResult" class="mt-4 hidden"></div>
                </div>

                <h4 class="font-bold mb-3">Meus Amigos (<span id="friendCount">0</span>)</h4>
                <div id="friendsList" class="space-y-3">
                    <p class="text-center text-gray-400 dark:text-gray-600 py-6">Nenhum amigo adicionado ainda</p>
                </div>

                <h4 class="font-bold mt-6 mb-3">Solicitações Recebidas (<span id="requestCount">0</span>)</h4>
                <div id="requestsList" class="space-y-3">
                    <p class="text-center text-gray-400 dark:text-gray-600 py-6">Nenhuma solicitação pendente</p>
                </div>
            </div>
        </div>

        <!-- Página: Histórico -->
        <div id="historyPage" class="page hidden">
            <div class="glass rounded-xl p-6">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-xl font-bold">Histórico de Cópias</h3>
                    <button id="exportAll" class="px-3 py-1 rounded-lg bg-primary/20 text-primary text-sm hover:bg-primary/30">
                        <i class="fa fa-download mr-1"></i> Exportar Tudo
                    </button>
                </div>
                <div id="historyList" class="space-y-3">
                    <p class="text-center text-gray-400 dark:text-gray-600 py-8">Nenhum histórico ainda</p>
                </div>
            </div>
        </div>

        <!-- Página: Configurações -->
        <div id="settingsPage" class="page hidden">
            <div class="glass rounded-xl p-6 space-y-6">
                <h3 class="text-xl font-bold">Configurações</h3>

                <div class="space-y-4">
                    <div class="flex justify-between items-center">
                        <div>
                            <h4 class="font-medium">Idioma</h4>
                            <p class="text-xs text-gray-400 dark:text-gray-600">Escolha o idioma do sistema</p>
                        </div>
                        <select id="language" class="px-3 py-2 rounded-lg bg-secondary/30 dark:bg-gray-100 border border-primary/30">
                            <option value="pt">Português</option>
                            <option value="en">English</option>
                        </select>
                    </div>

                    <div class="flex justify-between items-center">
                        <div>
                            <h4 class="font-medium">Volume dos Efeitos</h4>
                            <p class="text-xs text-gray-400 dark:text-gray-600">Ajuste o som dos botões</p>
                        </div>
                        <input type="range" id="soundVolume" min="0" max="100" value="70" class="w-32">
                    </div>

                    <div class="space-y-2">
                        <h4 class="font-medium">Cor Principal</h4>
                        <div class="flex gap-3 flex-wrap">
                            <button class="color-option w-8 h-8 rounded-full bg-purple-500 border-2 border-transparent" data-color="#7C3AED"></button>
                            <button class="color-option w-8 h-8 rounded-full bg-blue-500 border-2 border-transparent" data-color="#3B82F6"></button>
                            <button class="color-option w-8 h-8 rounded-full bg-green-500 border-2 border-transparent" data-color="#10B981"></button>
                            <button class="color-option w-8 h-8 rounded-full bg-pink-500 border-2 border-transparent" data-color="#EC4899"></button>
                            <button class="color-option w-8 h-8 rounded-full bg-orange-500 border-2 border-transparent" data-color="#F97316"></button>
                        </div>
                    </div>

                    <hr class="border-primary/20">

                    <button id="clearData" class="w-full py-3 rounded-lg bg-danger/20 text-danger hover:bg-danger/30 transition">
                        <i class="fa fa-trash mr-2"></i> Limpar Todos os Dados
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Compartilhar -->
    <div id="shareModal" class="fixed inset-0 z-[60] hidden flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
        <div class="glass rounded-xl p-6 max-w-md w-full">
            <h3 class="text-xl font-bold mb-4">Compartilhar Chave</h3>
            <p class="text-sm text-gray-400 mb-4">Escolha como quer compartilhar sua chave:</p>
            
            <div class="space-y-3 mb-6">
                <button id="shareCopy" class="w-full py-3 rounded-lg bg-secondary/50 hover:bg-secondary/70 transition flex items-center justify-center gap-2">
                    <i class="fa fa-copy"></i> Copiar Link de Acesso
                </button>
                <button id="shareFriend" class="w-full py-3 rounded-lg bg-primary hover:bg-primary/90 transition flex items-center justify-center gap-2">
                    <i class="fa fa-user-plus"></i> Enviar para Amigo
                </button>
            </div>

            <button id="closeShare" class="w-full py-2 rounded-lg bg-gray-700 hover:bg-gray-600 transition">Fechar</button>
        </div>
    </div>

    <!-- Modal QR Code -->
    <div id="qrModal" class="fixed inset-0 z-[60] hidden flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
        <div class="glass rounded-xl p-6 max-w-sm w-full text-center">
            <h3 class="text-xl font-bold mb-4">QR Code da Chave</h3>
            <div id="qrContainer" class="bg-white p-4 rounded-lg mb-4 inline-block"></div>
            <button id="closeQR" class="w-full py-2 rounded-lg bg-primary text-white">Fechar</button>
        </div>
    </div>

    <!-- Notificação -->
    <div id="toast" class="fixed bottom-6 left-1/2 transform -translate-x-1/2 z-[70] glass px-6 py-3 rounded-lg shadow-xl opacity-0 transition-opacity duration-300 pointer-events-none flex items-center gap-2">
        <i id="toastIcon" class="fa fa-check-circle text-success"></i>
        <span id="toastText">Mensagem</span>
    </div>

    <script>
        // ================== BANCO DE DADOS GLOBAL ==================
        const firebaseConfig = {
            apiKey: "AIzaSyB6XkfX7QZ9xLdM7Xz9Qw8eR7T6y5W4v3U2",
            authDomain: "h4x-system.firebaseapp.com",
            projectId: "h4x-system",
            storageBucket: "h4x-system.appspot.com",
            messagingSenderId: "1234567890",
            appId: "1:1234567890:web:abc123def456ghi789jkl01"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        // ================== VARIAVEIS GLOBAIS ==================
        let currentUser = null;
        let selectedDuration = "1h";
        let activeKey = null;
        let history = [];
        let totalCopies = 0;
        let totalKeys = 0;
        let soundVolume = 0.7;
        let friends = [];
        let requests = [];

        // ================== TELA DE CARREGAMENTO ==================
        async function initLoading() {
            const progress = document.getElementById("loadingProgress");
            const percent = document.getElementById("loadingPercent");
            for(let i = 0; i <= 100; i++) {
                progress.style.width = i + "%";
                percent.textContent = i + "%";
                await new Promise(r => setTimeout(r, 15));
            }
            setTimeout(() => {
                document.getElementById("loadingScreen").classList.add("opacity-0");
                setTimeout(() => {
                    document.getElementById("loadingScreen").classList.add("hidden");
                    checkSavedLogin();
                    initParticles();
                    updateDateTime();
                    setInterval(updateDateTime, 1000);
                }, 500);
            }, 800);
        }
        initLoading();

        // ================== FUNDO DE PARTÍCULAS ==================
        function initParticles() {
            const canvas = document.getElementById('particles');
            const ctx = canvas.getContext('2d');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            window.addEventListener('resize', () => { canvas.width = window.innerWidth; canvas.height = window.innerHeight; });
            
            const particles = [];
            for(let i=0; i<150; i++) particles.push({
                x: Math.random()*canvas.width, y: Math.random()*canvas.height,
                size: Math.random()*2+0.5, speedX: (Math.random()-0.5)*0.5, speedY: (Math.random()-0.5)*0.5
            });

            function animate() {
                ctx.clearRect(0,0,canvas.width,canvas.height);
                particles.forEach(p => {
                    p.x += p.speedX; p.y += p.speedY;
                    if(p.x<0) p.x=canvas.width; if(p.x>canvas.width) p.x=0;
                    if(p.y<0) p.y=canvas.height; if(p.y>canvas.height) p.y=0;
                    ctx.fillStyle = 'rgba(124, 58, 237, 0.6)';
                    ctx.beginPath(); ctx.arc(p.x,p.y,p.size,0,Math.PI*2); ctx.fill();
                });
                requestAnimationFrame(animate);
            }
            animate();
        }

        // ================== SISTEMA DE LOGIN ==================
        function checkSavedLogin() {
            const saved = localStorage.getItem("h4x_remember");
            if(saved) {
                const data = JSON.parse(saved);
                currentUser = data.user;
                showMainScreen();
                return;
            }
            document.getElementById("loginScreen").classList.remove("hidden");
        }

        // Força da senha
        document.getElementById("loginPass").addEventListener("input", e => {
            const val = e.target.value;
            let score = 0;
            if(val.length >=6) score++;
            if(val.length >=10) score++;
            if(/[A-Z]/.test(val)) score++;
            if(/[0-9]/.test(val)) score++;
            if(/[^A-Za-z0-9]/.test(val)) score++;

            const bar = document.getElementById("passStrength");
            const text = document.getElementById("strengthText");
            const widths = ['20%','40%','60%','80%','100%'];
            const colors = ['#EF4444','#F59E0B','#F59E0B','#10B981','#10B981'];
            const texts = ['Muito Fraca','Fraca','Média','Forte','Muito Forte'];
            
            bar.style.width = widths[score] || '0%';
            bar.style.backgroundColor = colors[score] || '#EF4444';
            text.textContent = texts[score] || 'Força da senha';
        });

        // Mostrar/ocultar senha
        document.getElementById("togglePass").addEventListener("click", () => {
            const pass = document.getElementById("loginPass");
            const btn = document.getElementById("togglePass");
            pass.type = pass.type === "password" ? "text" : "password";
            btn.innerHTML = pass.type === "password" ? '<i class="fa fa-eye-slash"></i> Ocultar' : '<i class="fa fa-eye"></i> Mostrar';
        });

        // Enviar login/cadastro
        document.getElementById("loginForm").addEventListener("submit", async e => {
            e.preventDefault();
            const user = document.getElementById("loginUser").value.trim();
            const pass = document.getElementById("loginPass").value;
            
            if(!user || !pass) return showToast("Preencha todos os campos!", "error");

            const userRef = db.collection("users").doc(user);
            const doc = await userRef.get();

            if(!doc.exists) {
                await userRef.set({
                    password: btoa(pass),
                    createdAt: new Date(),
                    lastLogin: new Date(),
                    friends: [],
                    requests: []
                });
                showToast("Conta criada com sucesso!");
            } else {
                if(atob(doc.data().password) !== pass) return showToast("Senha incorreta!", "error");
                await userRef.update({ lastLogin: new Date() });
            }

            currentUser = user;
            const lastLogin = new Date().toLocaleString('pt-BR');
            localStorage.setItem("h4x_lastLogin", lastLogin);

            if(document.getElementById("rememberMe").checked) {
                localStorage.setItem("h4x_remember", JSON.stringify({user: currentUser}));
            }

            showMainScreen();
            loadUserData();
        });

        function showMainScreen() {
            document.getElementById("loginScreen").classList.add("hidden");
            document.getElementById("mainContent").classList.remove("hidden");
            document.getElementById("userName").textContent = currentUser;
            document.getElementById("lastLogin").textContent = localStorage.getItem("h4x_lastLogin") || new Date().toLocaleDateString('pt-BR');
        }

        // ================== NAVEGAÇÃO ==================
        document.getElementById("openSidebar").addEventListener("click", () => {
            document.getElementById("sidebar").style.width = "260px";
            document.getElementById("sidebarOverlay").classList.remove("hidden");
        });
        document.getElementById("closeSidebar").addEventListener("click", closeSidebar);
        document.getElementById("sidebarOverlay").addEventListener("click", closeSidebar);
        function closeSidebar() {
            document.getElementById("sidebar").style.width = "0";
            document.getElementById("sidebarOverlay").classList.add("hidden");
        }

        document.querySelectorAll(".nav-btn").forEach(btn => {
            btn.addEventListener("click", () => {
                closeSidebar();
                const page = btn.dataset.page;
                document.querySelectorAll(".page").forEach(p => p.classList.add("hidden"));
                document.getElementById(page + "Page").classList.remove("hidden");
            });
        });

        // ================== GERAR CHAVES ==================
        document.querySelectorAll(".duration-btn").forEach(btn => {
            btn.addEventListener("click", () => {
                document.querySelectorAll(".duration-btn").forEach(b => b.classList.remove("border-primary"));
                btn.classList.add("border-primary");
                selectedDuration = btn.dataset.value;
            });
        });

        document.getElementById("generateKey").addEventListener("click", () => {
            const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
            let key = "H4X-";
            for(let i=0; i<16; i++) key += chars[Math.floor(Math.random()*chars.length)];
            
            activeKey = { value: key, duration: selectedDuration, createdAt: Date.now() };
            totalKeys++;
            saveUserData();

            document.getElementById("keyResult").classList.remove("hidden");
            document.getElementById("generatedKey").textContent = key;
            document.getElementById("generatedKey").style.animation = "none";
            setTimeout(() => document.getElementById("generatedKey").style.animation = "typing 2s steps(25) forwards", 10);
            
            updateKeyStatus();
            showToast("Chave gerada com sucesso!");
        });

        // Copiar chave
        document.getElementById("copyKey").addEventListener("click", async () => {
            if(!activeKey) return;
            await navigator.clipboard.writeText(activeKey.value);
            totalCopies++;
            document.getElementById("copyCount").textContent = totalCopies;
            
            history.unshift({ key: activeKey.value, time: new Date().toLocaleTimeString('pt-BR') });
            updateHistory();
            saveUserData();

            confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
            showToast("Chave copiada! 🎉");
        });

        // Exportar chave individual
        document.getElementById("exportKey").addEventListener("click", () => {
            if(!activeKey) return showToast("Gere uma chave primeiro!", "error");
            const dataStr = `CHAVE H4X SYSTEM\nValor: ${activeKey.value}\nValidade: ${selectedDuration === '1h' ? '1 Hora' : selectedDuration === '1d' ? '1 Dia' : 'Permanente'}\nGerada em: ${new Date().toLocaleString('pt-BR')}`;
            const blob = new Blob([dataStr], {type: 'text/plain'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `chave-${Date.now()}.txt`;
            a.click();
            URL.revokeObjectURL(url);
            showToast("Chave exportada com sucesso!");
        });

        // Exportar todas as chaves/histórico
        document.getElementById("exportAll").addEventListener("click", () => {
            if(history.length === 0) return showToast("Nenhum dado para exportar!", "error");
            const dataStr = "HISTÓRICO DE CHAVES - H4X SYSTEM\n\n" + history.map((item, i) => `${i+1}. ${item.key} - ${item.time}`).join('\n');
            const blob = new Blob([dataStr], {type: 'text/plain'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `historico-chaves-${new Date().toLocaleDateString('pt-BR').replace(/\//g,'-')}.txt`;
            a.click();
            URL.revokeObjectURL(url);
            showToast("Histórico exportado!");
        });

        // Compartilhar chave
        document.getElementById("shareKey").addEventListener("click", () => {
            if(!activeKey) return showToast("Gere uma chave primeiro!", "error");
            document.getElementById("shareModal").classList.remove("hidden");
        });

        document.getElementById("closeShare").addEventListener("click", () => {
            document.getElementById("shareModal").classList.add("hidden");
        });

        document.getElementById("shareCopy").addEventListener("click", async () => {
            const shareLink = `${window.location.origin}?key=${activeKey.value}`;
            await navigator.clipboard.writeText(shareLink);
            showToast("Link de compartilhamento copiado!");
            document.getElementById("shareModal").classList.add("hidden");
        });

        document.getElementById("shareFriend").addEventListener("click", async () => {
            if(friends.length === 0) return showToast("Adicione amigos primeiro!", "error");
            document.getElementById("shareModal").classList.add("hidden");
            showToast("Selecione um amigo na lista para enviar a chave!");
        });

        // QR Code
        document.getElementById("showQR").addEventListener("click", () => {
            if(!activeKey) return;
            document.getElementById("qrContainer").innerHTML = "";
            QRCode.toCanvas(document.createElement('canvas'), activeKey.value, { width: 180 }, (err, canvas) => {
                if(!err) document.getElementById("qrContainer").appendChild(canvas);
            });
            document.getElementById("qrModal").classList.remove("hidden");
        });
        document.getElementById("closeQR").addEventListener("click", () => {
            document.getElementById("qrModal").classList.add("hidden");
        });

        // ================== SISTEMA DE AMIGOS ==================
        document.getElementById("searchBtn").addEventListener("click", async () => {
            const search = document.getElementById("searchUser").value.trim();
            if(!search) return showToast("Digite um nome para pesquisar!", "error");
            if(search === currentUser) return showToast("Você não pode adicionar a si mesmo!", "error");

            const userRef = db.collection("users").doc(search);
            const doc = await userRef.get();
            const resultDiv = document.getElementById("searchResult");

            if(!doc.exists) {
                resultDiv.innerHTML = `<p class="text-sm text-danger">Usuário não encontrado ou não usa o sistema!</p>`;
                resultDiv.classList.remove("hidden");
                return;
            }

            resultDiv.innerHTML = `
                <div class="friend-card glass rounded-lg p-3 flex items-center justify-between">
                    <div class="flex items-center gap-3">
                        <i class="fa fa-user-circle text-2xl text-primary"></i>
                        <div>
                            <p class="font-bold">${search}</p>
                            <p class="text-xs text-gray-400">Usa o H4X System</p>
                        </div>
                    </div>
                    <button class="add-friend-btn px-3 py-1 rounded-lg bg-primary text-white text-sm hover:bg-primary/90" data-user="${search}">
                        Adicionar
                    </button>
                </div>
            `;
            resultDiv.classList.remove("hidden");

            document.querySelector(".add-friend-btn").addEventListener("click", async (e) => {
                const targetUser = e.target.dataset.user;
                await db.collection("users").doc(targetUser).update({
                    requests: firebase.firestore.FieldValue.arrayUnion(currentUser)
                });
                showToast("Solicitação enviada para " + targetUser + "!");
                resultDiv.innerHTML = `<p class="text-sm text-success">Solicitação enviada!</p>`;
            });
        });

        async function loadFriendsData() {
            const userDoc = await db.collection("users").doc(currentUser).get();
            const data = userDoc.data();
            friends = data.friends || [];
            requests = data.requests || [];
            
            updateFriendsList();
            updateRequestsList();
            document.getElementById("totalFriends").textContent = friends.length;
        }

        function updateFriendsList() {
            const list = document.getElementById("friendsList");
            document.getElementById("friendCount").textContent = friends.length;
            
            if(friends.length === 0) {
                list.innerHTML = `<p class="text-center text-gray-400 dark:text-gray-600 py-6">Nenhum amigo adicionado ainda</p>`;
                return;
            }

            list.innerHTML = friends.map(friend => `
                <div class="friend-card glass rounded-lg p-3 flex items-center justify-between">
                    <div class="flex items-center gap-3">
                        <i class="fa fa-user-circle text-2xl text-primary"></i>
                        <span class="font-bold">${friend}</span>
                    </div>
                    <div class="flex gap-2">
                        <button class="share-to-friend p-2 rounded-lg bg-success/20 text-success hover:bg-success/30" data-friend="${friend}" title="Enviar chave">
                            <i class="fa fa-paper-plane"></i>
                        </button>
                        <button class="remove-friend p-2 rounded-lg bg-danger/20 text-danger hover:bg-danger/30" data-friend="${friend}" title="Remover amigo">
                            <i class="fa fa-trash"></i>
                        </button>
                    </div>
                </div>
            `).join('');

            document.querySelectorAll(".share-to-friend").forEach(btn => {
                btn.addEventListener("click", async () => {
                    if(!activeKey) return showToast("Gere uma chave primeiro!", "error");
                    const friend = btn.dataset.friend;
                    showToast(`Chave enviada para ${friend}!`);
                });
            });

            document.querySelectorAll(".remove-friend").forEach(btn => {
                btn.addEventListener("click", async () => {
                    const friend = btn.dataset.friend;
                    await db.collection("users").doc(currentUser).update({
                        friends: firebase.firestore.FieldValue.arrayRemove(friend)
                    });
                    await db.collection("users").doc(friend).update({
                        friends: firebase.firestore.FieldValue.arrayRemove(currentUser)
                    });
                    showToast("Amigo removido!");
                    loadFriendsData();
                });
            });
        }

        function updateRequestsList() {
            const list = document.getElementById("requestsList");
            document.getElementById("requestCount").textContent = requests.length;
            
            if(requests.length === 0) {
                list.innerHTML = `<p class="text-center text-gray-400 dark:text-gray-600 py-6">Nenhuma solicitação pendente</p>`;
                return;
            }

            list.innerHTML = requests.map(user => `
                <div class="friend-card glass rounded-lg p-3 flex items-center justify-between">
                    <div class="flex items-center gap-3">
                        <i class="fa fa-user-plus text-2xl text-warning"></i>
                        <span class="font-bold">${user}</span>
                    </div>
                    <div class="flex gap-2">
                        <button class="accept-request p-2 rounded-lg bg-success/20 text-success hover:bg-success/30" data-user="${user}">
                            <i class="fa fa-check"></i>
                        </button>
                        <button class="reject-request p-2 rounded-lg bg-danger/20 text-danger hover:bg-danger/30" data-user="${user}">
                            <i class="fa fa-times"></i>
                        </button>
                    </div>
                </div>
            `).join('');

            document.querySelectorAll(".accept-request").forEach(btn => {
                btn.addEventListener("click", async () => {
                    const user = btn.dataset.user;
                    await db.collection("users").doc(currentUser).update({
                        friends: firebase.firestore.FieldValue.arrayUnion(user),
                        requests: firebase.firestore.FieldValue.arrayRemove(user)
                    });
                    await db.collection("users").doc(user).update({
                        friends: firebase.firestore.FieldValue.arrayUnion(currentUser)
                    });
                    showToast("Solicitação aceita!");
                    loadFriendsData();
                });
            });

            document.querySelectorAll(".reject-request").forEach(btn => {
                btn.addEventListener("click", async () => {
                    const user = btn.dataset.user;
                    await db.collection("users").doc(currentUser).update({
                        requests: firebase.firestore.FieldValue.arrayRemove(user)
                    });
                    showToast("Solicitação recusada!");
                    loadFriendsData();
                });
            });
        }

        // ================== FUNÇÕES GERAIS ==================
        function updateDateTime() {
            const now = new Date();
            document.getElementById("currentDateTime").textContent = now.toLocaleString('pt-BR');
        }

        function updateKeyStatus() {
            if(!activeKey) return;
            document.getElementById("totalKeys").textContent = totalKeys;

            if(activeKey.duration === "perm") {
                document.getElementById("keyStatus").textContent = "Ativa Permanentemente";
                document.getElementById("keyStatus").className = "text-sm font-bold text-success";
                document.getElementById("validityProgress").style.width = "100%";
                document.getElementById("timeRemaining").textContent = "Nunca expira";
            } else {
            const durationMs = activeKey.duration === "1h" ? 3600000 : 86400000;
            const endTime = activeKey.createdAt + durationMs;
            const remaining = endTime - Date.now();
            const percent = Math.max(0, (remaining/durationMs)*100);
            
            document.getElementById("validityProgress").style.width = percent + "%";
            document.getElementById("timeRemaining").textContent = formatTime(remaining);
            
            if(remaining <= 0) {
                document.getElementById("keyStatus").textContent = "Expirada";
                document.getElementById("keyStatus").className = "text-sm font-bold text-danger";
            } else {
                document.getElementById("keyStatus").textContent = "Ativa";
                document.getElementById("keyStatus").className = "text-sm font-bold text-success";
            }
        }
        setInterval(updateKeyStatus, 1000);
    }

    function formatTime(ms) {
        if(ms <=0) return "Expirada";
        const h = Math.floor(ms / 3600000);
        const m = Math.floor((ms % 3600000)/60000);
        const s = Math.floor((ms % 60000)/1000);
        return `${h}h ${m}m ${s}s restantes`;
    }

    function updateHistory() {
        const list = document.getElementById("historyList");
        if(history.length ===0) {
            list.innerHTML = '<p class="text-center text-gray-400 dark:text-gray-600 py-8">Nenhum histórico ainda</p>';
            return;
        }
        list.innerHTML = history.map((item, i) => `
            <div class="flex justify-between items-center p-3 rounded-lg bg-secondary/30 dark:bg-gray-100 animate-bounce-in" style="animation-delay: ${i*0.1}s">
                <span class="font-mono text-sm">${item.key.slice(0,10)}••••••</span>
                <span class="text-xs text-gray-400">${item.time}</span>
            </div>
        `).join('');
    }

    // ================== CONFIGURAÇÕES E SALVAMENTO ==================
    document.getElementById("themeToggle").addEventListener("click", () => {
        document.documentElement.classList.toggle("dark");
        localStorage.setItem("h4x_theme", document.documentElement.classList.contains("dark") ? "dark" : "light");
    });
    if(localStorage.getItem("h4x_theme") === "dark") document.documentElement.classList.add("dark");

    document.querySelectorAll(".color-option").forEach(btn => {
        btn.addEventListener("click", () => {
            document.querySelectorAll(".color-option").forEach(b => b.classList.remove("border-white"));
            btn.classList.add("border-white");
            document.documentElement.style.setProperty('--primary', btn.dataset.color);
            localStorage.setItem("h4x_color", btn.dataset.color);
        });
    });
    if(localStorage.getItem("h4x_color")) {
        document.documentElement.style.setProperty('--primary', localStorage.getItem("h4x_color"));
        document.querySelector(`[data-color="${localStorage.getItem("h4x_color")}"]`)?.classList.add("border-white");
    }

    document.getElementById("soundVolume").addEventListener("input", e => {
        soundVolume = e.target.value / 100;
        localStorage.setItem("h4x_volume", e.target.value);
    });
    if(localStorage.getItem("h4x_volume")) {
        document.getElementById("soundVolume").value = localStorage.getItem("h4x_volume");
        soundVolume = localStorage.getItem("h4x_volume") / 100;
    }

    document.getElementById("language").addEventListener("change", e => {
        localStorage.setItem("h4x_lang", e.target.value);
    });
    if(localStorage.getItem("h4x_lang")) {
        document.getElementById("language").value = localStorage.getItem("h4x_lang");
    }

    document.getElementById("clearData").addEventListener("click", () => {
        if(confirm("Tem certeza? Todos os dados locais serão apagados!")) {
            localStorage.clear();
            location.reload();
        }
    });

    document.getElementById("logoutBtn").addEventListener("click", () => {
        localStorage.removeItem("h4x_remember");
        location.reload();
    });

    function saveUserData() {
        localStorage.setItem(`h4x_data_${currentUser}`, JSON.stringify({
            activeKey, history, totalCopies, totalKeys
        }));
    }

    async function loadUserData() {
        const data = localStorage.getItem(`h4x_data_${currentUser}`);
        if(data) {
            const parsed = JSON.parse(data);
            activeKey = parsed.activeKey;
            history = parsed.history || [];
            totalCopies = parsed.totalCopies || 0;
            totalKeys = parsed.totalKeys || 0;
            updateHistory();
            updateKeyStatus();
        }
        await loadFriendsData();
    }

    // Notificações
    function showToast(msg, type="success") {
        const t = document.getElementById("toast");
        document.getElementById("toastText").textContent = msg;
        document.getElementById("toastIcon").className = `fa fa-${type === "success" ? "check-circle text-success" : "exclamation-circle text-danger"}`;
        t.classList.remove("opacity-0");
        setTimeout(() => t.classList.add("opacity-0"), 3500);
    }
</script>
</body>
</html>
