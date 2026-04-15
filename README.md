
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Cyber Security Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .view { display: none; }
        .view.active { display: flex; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .cyber-alert { animation: pulse-red 2s infinite; }
        @keyframes pulse-red { 0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.4); } 70% { box-shadow: 0 0 0 10px rgba(239, 68, 68, 0); } 100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); } }
    </style>
</head>
<body class="bg-slate-100 min-h-screen">

    <div class="fixed top-6 right-6 z-[200]">
        <div class="relative inline-block text-left">
            <button onclick="toggleLangMenu()" class="bg-white border shadow-lg rounded-full p-3 flex items-center gap-2 hover:bg-gray-50 transition">
                <i class="fas fa-globe-americas text-cyan-600 text-xl"></i>
                <span id="current-lang-name" class="text-xs font-bold text-slate-700 hidden md:block">Português (Brasil)</span>
            </button>
            <div id="lang-menu" class="hidden absolute right-0 mt-2 w-48 bg-white rounded-2xl shadow-2xl border border-slate-100 overflow-hidden">
                <button onclick="changeLang('pt')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇧🇷 Português</button>
                <button onclick="changeLang('en')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇺🇸 English</button>
                <button onclick="changeLang('es')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇪🇸 Español</button>
            </div>
        </div>
    </div>

    <div id="login-view" class="view active items-center justify-center min-h-screen bg-slate-900 p-4 relative">
        <div id="lockdown-overlay" class="hidden absolute inset-0 bg-black/80 z-[300] flex flex-col items-center justify-center text-white p-6 text-center">
            <i class="fas fa-shield-virus text-6xl text-red-500 mb-4 cyber-alert"></i>
            <h2 class="text-2xl font-black uppercase">Sistema Bloqueado</h2>
            <p class="text-slate-400 mt-2">Muitas tentativas falhas. Aguarde 30 segundos.</p>
            <div id="timer" class="mt-4 text-4xl font-mono font-bold text-cyan-500">30s</div>
        </div>

        <div class="bg-white p-8 md:p-12 rounded-[3rem] shadow-2xl w-full max-w-md text-center border-b-8 border-cyan-500">
            <div class="mb-10 text-center">
                <i class="fas fa-fingerprint text-6xl text-cyan-500 mb-4"></i>
                <h1 class="text-4xl font-black tracking-tighter italic text-slate-900">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
                <p id="txt-login-subtitle" class="text-slate-400 text-[10px] font-bold tracking-widest uppercase mt-2 italic">Cyber Security Protocol Active</p>
            </div>
            <div class="space-y-4">
                <button onclick="switchView('cliente')" id="btn-client" class="w-full bg-slate-100 text-slate-800 font-black py-5 rounded-2xl hover:bg-slate-200 transition uppercase text-xs">SOU CLIENTE</button>
                <div class="relative py-4"><hr><span id="txt-admin-label" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-4 text-[9px] font-black text-slate-400 uppercase tracking-widest">Acesso Administrativo</span></div>
                <input type="password" id="login-pass" class="w-full p-5 bg-slate-50 border border-slate-100 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center font-bold" placeholder="••••••••">
                <button onclick="checkLogin()" id="btn-admin" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl hover:bg-cyan-600 shadow-2xl transition uppercase text-xs">VERIFICAR IDENTIDADE</button>
            </div>
            <p id="login-error" class="text-red-500 text-[10px] mt-6 hidden font-black uppercase tracking-tighter">Acesso Negado: Credenciais Inválidas</p>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row min-h-screen">
        <aside class="w-full md:w-72 bg-slate-900 text-white flex flex-col p-6 shadow-2xl">
            <div class="mb-10 flex items-center gap-3">
                <i class="fas fa-shield-halved text-cyan-400"></i>
                <h2 class="text-2xl font-black italic">RUAN <span class="text-cyan-400">ADM</span></h2>
            </div>
            <nav class="flex-1 space-y-3">
                <button onclick="switchTab('dashboard')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition active" id="btn-tab-dashboard"><i class="fas fa-chart-line"></i> <span class="txt-nav-dash">Dashboard</span></button>
                <button onclick="switchTab('os')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition" id="btn-tab-os"><i class="fas fa-wrench"></i> <span class="txt-nav-os">Ordens</span></button>
                <button onclick="switchTab('estoque')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition" id="btn-tab-estoque"><i class="fas fa-store"></i> <span class="txt-nav-stock">Mercado</span></button>
            </nav>
            <button onclick="logout()" class="p-4 text-red-400 font-black flex items-center gap-3 mt-auto rounded-2xl hover:bg-red-500/10 transition">
                <i class="fas fa-power-off"></i> <span class="txt-logout">SAIR</span>
            </button>
        </aside>
        
        <main class="flex-1 p-6 md:p-12 overflow-y-auto">
            <div id="tab-dashboard" class="tab-content active space-y-8">
                <div class="bg-cyan-100/50 border border-cyan-200 p-4 rounded-2xl flex items-center gap-4 text-cyan-700 text-xs font-bold">
                    <i class="fas fa-user-shield"></i>
                    <span>Sessão Protegida: Criptografia de Ponta a Ponta Ativa</span>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm"><p class="text-slate-400 text-xs font-black uppercase mb-2">Total O.S</p><h3 id="dash-os" class="text-5xl font-black text-slate-900">0</h3></div>
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm"><p class="text-slate-400 text-xs font-black uppercase mb-2">Itens Estoque</p><h3 id="dash-stock" class="text-5xl font-black text-cyan-600">0</h3></div>
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm border-2 border-red-50"><p class="text-red-400 text-xs font-black uppercase mb-2">Alertas</p><h3 id="dash-alert" class="text-5xl font-black text-red-500">0</h3></div>
                </div>
            </div>

            <div id="tab-os" class="tab-content space-y-6">
                <div class="bg-white p-8 rounded-[2.5rem] shadow-sm">
                    <h3 class="font-black mb-6 uppercase text-sm">Nova Ordem de Serviço</h3>
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <input type="text" id="os-cliente" placeholder="Nome do Cliente" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500">
                        <input type="number" id="os-whats" placeholder="WhatsApp" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500">
                        <input type="text" id="os-aparelho" placeholder="Equipamento" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500">
                        <button onclick="addOS()" class="bg-cyan-600 text-white font-black rounded-2xl uppercase text-xs shadow-lg shadow-cyan-200">Criar OS</button>
                    </div>
                </div>
                <div class="bg-white rounded-[2.5rem] shadow-sm overflow-hidden"><table class="w-full text-left"><thead class="bg-slate-50 text-[10px] uppercase font-black text-slate-400 border-b"><tr><th class="p-6">ID</th><th class="p-6">Cliente</th><th class="p-6">Aparelho</th><th class="p-6">Status</th><th class="p-6 text-right">Ações</th></tr></thead><tbody id="osTableBody" class="divide-y divide-slate-50"></tbody></table></div>
            </div>

            <div id="tab-estoque" class="tab-content space-y-6">
                <div class="bg-white p-8 rounded-[2.5rem] shadow-sm">
                    <h3 class="font-black mb-6 uppercase text-sm text-cyan-600">Entrada de Mercadoria</h3>
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <input type="text" id="p-nome" placeholder="Produto" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500">
                        <input type="number" id="p-qtd" placeholder="Qtd" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500">
                        <input type="number" step="0.01" id="p-preco" placeholder="Preço" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500">
                        <button onclick="addProduto()" class="bg-slate-900 text-white font-black rounded-2xl uppercase text-xs">Salvar</button>
                    </div>
                </div>
                <div class="bg-white rounded-[2.5rem] shadow-sm overflow-hidden"><table class="w-full text-left"><thead class="bg-slate-50 text-[10px] uppercase font-black text-slate-400 border-b"><tr><th class="p-6">Produto</th><th class="p-6 text-center">Qtd</th><th class="p-6">Preço</th><th class="p-6 text-right">Controle</th></tr></thead><tbody id="prodTableBody" class="divide-y divide-slate-50"></tbody></table></div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col min-h-screen">
        <header class="bg-white p-6 shadow-sm border-b sticky top-0 z-50 flex flex-col md:flex-row gap-6 justify-between items-center">
            <h1 class="text-2xl font-black italic">RUAN <span class="text-cyan-500 font-black">STORE</span></h1>
            <div class="flex-1 max-w-2xl w-full">
                <div class="relative"><i class="fas fa-search absolute left-5 top-1/2 -translate-y-1/2 text-slate-300"></i><input type="text" id="searchBar" onkeyup="renderAll()" class="w-full p-5 pl-14 bg-slate-50 rounded-[1.5rem] border-none ring-1 ring-slate-100 focus:ring-2 focus:ring-cyan-500 outline-none transition" placeholder="O que você procura?"></div>
            </div>
            <div class="flex gap-4">
                <button onclick="openCart()" class="relative p-4 bg-slate-900 text-white rounded-2xl shadow-xl"><i class="fas fa-shopping-basket"></i><span id="cart-count" class="absolute -top-2 -right-2 bg-cyan-500 text-white text-[10px] font-black px-2 py-0.5 rounded-full border-2 border-white">0</span></button>
                <button onclick="logout()" class="p-4 bg-slate-50 rounded-2xl text-slate-400"><i class="fas fa-times"></i></button>
            </div>
        </header>
        <main class="container mx-auto p-8"><div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8"></div></main>
    </div>

    <div id="cart-modal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-md z-[300] hidden items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 shadow-2xl">
            <div class="flex justify-between items-center mb-8 border-b pb-6">
                <h2 id="txt-cart-title" class="text-3xl font-black italic">Carrinho</h2>
                <button onclick="closeCart()" class="text-4xl text-slate-200 hover:text-slate-400">&times;</button>
            </div>
            <div id="cart-items" class="space-y-4 mb-8 max-h-60 overflow-y-auto px-2"></div>
            <div class="pt-6 space-y-6">
                <div class="flex justify-between items-end font-black"><span class="text-slate-400 text-xs uppercase tracking-widest">Total</span><span id="cart-total" class="text-3xl text-cyan-600">R$ 0,00</span></div>
                <div class="grid grid-cols-2 gap-4">
                    <input type="time" id="c-hora" class="p-5 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none font-bold">
                    <select id="c-pgto" class="p-5 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none font-bold text-sm"><option>Pix</option><option>Dinheiro</option><option>Cartão</option></select>
                </div>
                <button onclick="finalizarPedido()" id="btn-finish" class="w-full bg-green-500 text-white font-black py-6 rounded-3xl shadow-xl uppercase tracking-widest text-xs">Finalizar Pedido</button>
            </div>
        </div>
    </div>

    <script>
        const SENHA_ADM = "admin123";
        let tentativasLogin = 0;
        let bloqueado = false;
        
        let ordens = JSON.parse(localStorage.getItem('assist_os')) || [];
        let produtos = JSON.parse(localStorage.getItem('assist_pr')) || [];
        let carrinho = [];
        let lang = 'pt';

        const dict = {
            pt: { name: "Português (Brasil)", client: "SOU CLIENTE", admin: "VERIFICAR IDENTIDADE", subtitle: "CYBER SECURITY PROTOCOL ACTIVE", admLabel: "ACESSO ADMINISTRATIVO", dash: "Dashboard", os: "Ordens", stock: "Mercado", logout: "SAIR", search: "O que você procura?", cart: "Meu Carrinho", pick: "PEGAR", finish: "FINALIZAR PEDIDO" },
            en: { name: "English (USA)", client: "I'M CUSTOMER", admin: "VERIFY IDENTITY", subtitle: "CYBER SECURITY PROTOCOL ACTIVE", admLabel: "ADMINISTRATIVE ACCESS", dash: "Dashboard", os: "Orders", stock: "Market", logout: "LOGOUT", search: "Search products...", cart: "My Cart", pick: "GET IT", finish: "FINISH ORDER" },
            es: { name: "Español (ES)", client: "SOY CLIENTE", admin: "VERIFICAR IDENTIDAD", subtitle: "CYBER SECURITY PROTOCOL ACTIVE", admLabel: "ACCESO ADMINISTRATIVO", dash: "Tablero", os: "Órdenes", stock: "Mercado", logout: "SALIR", search: "¿Qué estás buscando?", cart: "Mi Carrito", pick: "COMPRAR", finish: "FINALIZAR PEDIDO" }
        };

        // SEGURANÇA: SANITIZAÇÃO DE ENTRADA (XSS PROTECTION)
        function sanitize(str) {
            const div = document.createElement('div');
            div.textContent = str;
            return div.innerHTML;
        }

        // SEGURANÇA: LOGIN COM LOCKDOWN
        function checkLogin() {
            if(bloqueado) return;
            const pass = document.getElementById('login-pass').value;
            
            if(pass === SENHA_ADM) {
                tentativasLogin = 0;
                console.log("%c[SEGURANÇA] Acesso autorizado em: " + new Date().toLocaleString(), "color: green; font-weight: bold;");
                switchView('admin');
            } else {
                tentativasLogin++;
                document.getElementById('login-error').classList.remove('hidden');
                console.warn("[SEGURANÇA] Tentativa de acesso inválida ("+tentativasLogin+"/3)");
                
                if(tentativasLogin >= 3) {
                    ativarLockdown();
                }
            }
        }

        function ativarLockdown() {
            bloqueado = true;
            document.getElementById('lockdown-overlay').classList.remove('hidden');
            let tempo = 30;
            const int = setInterval(() => {
                tempo--;
                document.getElementById('timer').innerText = tempo + "s";
                if(tempo <= 0) {
                    clearInterval(int);
                    bloqueado = false;
                    tentativasLogin = 0;
                    document.getElementById('lockdown-overlay').classList.add('hidden');
                }
            }, 1000);
        }

        // LÓGICA DE IDIOMA E NAVEGAÇÃO
        function toggleLangMenu() { document.getElementById('lang-menu').classList.toggle('hidden'); }
        function changeLang(l) {
            lang = l;
            document.getElementById('current-lang-name').innerText = dict[l].name;
            document.getElementById('btn-client').innerText = dict[l].client;
            document.getElementById('btn-admin').innerText = dict[l].admin;
            document.getElementById('txt-login-subtitle').innerText = dict[l].subtitle;
            document.getElementById('txt-admin-label').innerText = dict[l].admLabel;
            document.getElementById('searchBar').placeholder = dict[l].search;
            document.getElementById('txt-cart-title').innerText = dict[l].cart;
            document.getElementById('btn-finish').innerText = dict[l].finish;
            document.querySelectorAll('.txt-nav-dash').forEach(e => e.innerText = dict[l].dash);
            document.querySelectorAll('.txt-nav-os').forEach(e => e.innerText = dict[l].os);
            document.querySelectorAll('.txt-nav-stock').forEach(e => e.innerText = dict[l].stock);
            document.querySelectorAll('.txt-logout').forEach(e => e.innerText = dict[l].logout);
            document.getElementById('lang-menu').classList.add('hidden');
            renderAll();
        }

        function switchView(v) { document.querySelectorAll('.view').forEach(e => e.classList.remove('active')); document.getElementById(v + '-view').classList.add('active'); renderAll(); }
        function logout() { location.reload(); }
        function switchTab(t) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active'));
            document.getElementById('tab-' + t).classList.add('active');
            document.getElementById('btn-tab-' + t).classList.add('active');
        }

        // FUNÇÕES ADM COM SANITIZAÇÃO
        function save() { localStorage.setItem('assist_os', JSON.stringify(ordens)); localStorage.setItem('assist_pr', JSON.stringify(produtos)); renderAll(); }

        function addOS() {
            const cliente = sanitize(document.getElementById('os-cliente').value);
            const whats = sanitize(document.getElementById('os-whats').value);
            const aparelho = sanitize(document.getElementById('os-aparelho').value);
            if(!cliente || !whats) return alert("Preencha os campos obrigatórios!");
            ordens.unshift({ id: Math.floor(Math.random()*9000)+1000, cliente, whats, aparelho, status: 'Pendente' });
            document.getElementById('os-cliente').value = ''; document.getElementById('os-whats').value = ''; document.getElementById('os-aparelho').value = '';
            save();
        }

        function addProduto() {
            const nome = sanitize(document.getElementById('p-nome').value);
            const qtd = parseInt(document.getElementById('p-qtd').value);
            const preco = parseFloat(document.getElementById('p-preco').value);
            if(!nome || isNaN(qtd)) return alert("Dados de produto inválidos!");
            produtos.push({ id: Date.now(), nome, qtd, preco });
            document.getElementById('p-nome').value = ''; document.getElementById('p-qtd').value = ''; document.getElementById('p-preco').value = '';
            save();
        }

        function renderAll() {
            const search = (document.getElementById('searchB
