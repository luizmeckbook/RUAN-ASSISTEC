
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Global System v5.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .view { display: none; }
        .view.active { display: flex; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
        .lockdown-active { pointer-events: none; opacity: 0.7; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen">

    <div class="fixed top-6 right-6 z-[200]">
        <div class="relative inline-block text-left">
            <button onclick="toggleLangMenu()" class="bg-white border shadow-lg rounded-full p-3 flex items-center gap-2 hover:bg-gray-50 transition">
                <i class="fas fa-globe-americas text-cyan-600 text-xl"></i>
                <span id="current-lang-name" class="text-xs font-bold text-slate-700 hidden md:block">Português (Brasil)</span>
            </button>
            <div id="lang-menu" class="hidden absolute right-0 mt-2 w-48 bg-white rounded-2xl shadow-2xl border border-slate-100 overflow-hidden bg-white">
                <button onclick="changeLang('pt')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇧🇷 Português</button>
                <button onclick="changeLang('en')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇺🇸 English</button>
                <button onclick="changeLang('es')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇪🇸 Español</button>
            </div>
        </div>
    </div>

    <div id="login-view" class="view active items-center justify-center min-h-screen bg-slate-900 p-4 relative">
        <div id="lockdown-overlay" class="hidden absolute inset-0 bg-black/90 z-[300] flex flex-col items-center justify-center text-white text-center">
            <i class="fas fa-user-shield text-6xl text-red-500 mb-4 animate-pulse"></i>
            <h2 class="text-2xl font-black uppercase">Segurança Ativada</h2>
            <p class="text-slate-400 mt-2">Muitas tentativas falhas. Bloqueio temporário.</p>
            <div id="timer" class="mt-4 text-4xl font-mono text-cyan-500">30s</div>
        </div>

        <div class="bg-white p-8 md:p-12 rounded-[3rem] shadow-2xl w-full max-w-md text-center">
            <div class="mb-10">
                <div class="bg-cyan-500 w-20 h-20 rounded-3xl flex items-center justify-center mx-auto mb-6 shadow-lg shadow-cyan-500/20">
                    <i class="fas fa-microchip text-4xl text-white"></i>
                </div>
                <h1 class="text-3xl font-black tracking-tighter italic">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
                <p id="txt-login-subtitle" class="text-slate-400 text-[10px] font-bold tracking-widest uppercase mt-2 italic">Acesso Restrito</p>
            </div>
            
            <div class="space-y-4" id="login-container">
                <button onclick="switchView('cliente')" id="btn-client" class="w-full bg-slate-100 text-slate-800 font-black py-5 rounded-2xl hover:bg-slate-200 transition uppercase text-xs tracking-widest">Acesso Cliente</button>
                <div class="relative py-4"><hr><span id="txt-admin-label" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-4 text-[9px] font-black text-slate-300 uppercase">Painel ADM</span></div>
                <input type="password" id="login-pass" class="w-full p-5 bg-slate-50 border border-slate-100 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center font-bold" placeholder="••••••••">
                <button onclick="checkLogin()" id="btn-admin" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl hover:bg-cyan-600 transition uppercase text-xs">Autenticar ADM</button>
                <p id="login-error" class="text-red-500 text-[10px] hidden font-black uppercase">Senha Inválida</p>
            </div>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row min-h-screen">
        <aside class="w-full md:w-72 bg-slate-900 text-white flex flex-col p-6 shadow-2xl">
            <h2 class="text-2xl font-black italic mb-10 tracking-tighter">RUAN <span class="text-cyan-400">ADM</span></h2>
            <nav class="flex-1 space-y-2">
                <button onclick="switchTab('dashboard')" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold transition active" id="nav-dashboard"><i class="fas fa-chart-pie"></i> <span class="txt-nav-dash">Dashboard</span></button>
                <button onclick="switchTab('os')" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold transition" id="nav-os"><i class="fas fa-clipboard-list"></i> <span class="txt-nav-os">Ordens</span></button>
                <button onclick="switchTab('estoque')" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold transition" id="nav-estoque"><i class="fas fa-boxes-stacked"></i> <span class="txt-nav-stock">Mercado</span></button>
            </nav>
            <button onclick="logout()" class="p-4 text-red-400 font-black flex items-center gap-3 mt-auto hover:bg-red-500/10 rounded-xl transition">
                <i class="fas fa-sign-out-alt"></i> <span class="txt-logout">SAIR</span>
            </button>
        </aside>

        <main class="flex-1 p-6 md:p-12 overflow-y-auto">
            <div id="tab-dashboard" class="tab-content active space-y-8">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-b-4 border-slate-200"><p class="text-slate-400 text-[10px] font-black uppercase mb-2">Ordens Ativas</p><h3 id="dash-os" class="text-5xl font-black">0</h3></div>
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-b-4 border-cyan-500"><p class="text-slate-400 text-[10px] font-black uppercase mb-2">Produtos</p><h3 id="dash-stock" class="text-5xl font-black text-cyan-600">0</h3></div>
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-b-4 border-red-400"><p class="text-red-400 text-[10px] font-black uppercase mb-2">Alertas</p><h3 id="dash-alert" class="text-5xl font-black text-red-500">0</h3></div>
                </div>
            </div>

            <div id="tab-os" class="tab-content space-y-6">
                <div class="bg-white p-8 rounded-3xl shadow-sm">
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <input type="text" id="os-cliente" placeholder="Nome do Cliente" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200 focus:ring-2 focus:ring-cyan-500">
                        <input type="text" id="os-whats" placeholder="WhatsApp" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                        <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                        <button onclick="addOS()" class="bg-cyan-600 text-white font-black rounded-xl uppercase text-xs">Abrir Chamado</button>
                    </div>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden"><table class="w-full text-left"><thead class="bg-slate-50 text-[10px] font-black text-slate-400 uppercase"><tr><th class="p-6">Cliente</th><th class="p-6">Aparelho</th><th class="p-6">Status</th><th class="p-6 text-right">Ações</th></tr></thead><tbody id="osTableBody" class="divide-y"></tbody></table></div>
            </div>

            <div id="tab-estoque" class="tab-content space-y-6">
                <div class="bg-white p-8 rounded-3xl shadow-sm">
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <input type="text" id="p-nome" placeholder="Produto" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                        <input type="number" id="p-qtd" placeholder="Qtd" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                        <input type="number" step="0.01" id="p-preco" placeholder="Preço R$" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                        <button onclick="addProduto()" class="bg-slate-900 text-white font-black rounded-xl uppercase text-xs">Cadastrar</button>
                    </div>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden"><table class="w-full text-left"><thead class="bg-slate-50 text-[10px] font-black text-slate-400 uppercase"><tr><th class="p-6">Produto</th><th class="p-6">Qtd</th><th class="p-6">Preço</th><th class="p-6 text-right">Controle</th></tr></thead><tbody id="prodTableBody" class="divide-y"></tbody></table></div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col min-h-screen">
        <header class="bg-white p-6 shadow-sm border-b sticky top-0 z-50 flex flex-col md:flex-row gap-6 justify-between items-center">
            <h1 class="text-2xl font-black italic">RUAN <span class="text-cyan-500">STORE</span></h1>
            <input type="text" id="searchBar" onkeyup="renderAll()" class="flex-1 max-w-xl p-4 bg-slate-50 rounded-2xl outline-none ring-1 ring-slate-100 focus:ring-2 focus:ring-cyan-500" placeholder="Procurar no mercado...">
            <div class="flex gap-4">
                <button onclick="openCart()" class="relative p-4 bg-slate-900 text-white rounded-2xl">
                    <i class="fas fa-shopping-basket"></i>
                    <span id="cart-count" class="absolute -top-2 -right-2 bg-cyan-500 text-white text-[10px] font-black px-2 py-0.5 rounded-full border-2 border-white">0</span>
                </button>
                <button onclick="logout()" class="p-4 bg-slate-50 rounded-2xl text-slate-400 hover:text-red-500"><i class="fas fa-times"></i></button>
            </div>
        </header>
        <main class="container mx-auto p-8"><div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6"></div></main>
    </div>

    <div id="cart-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-[300] hidden items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 shadow-2xl">
            <h2 id="txt-cart-title" class="text-2xl font-black italic mb-6">Meu Carrinho</h2>
            <div id="cart-items" class="space-y-3 mb-6 max-h-60 overflow-y-auto px-1"></div>
            <div class="border-t pt-6 space-y-4">
                <div class="flex justify-between items-center font-black"><span>Total</span><span id="cart-total" class="text-2xl text-cyan-600">R$ 0,00</span></div>
                <div class="grid grid-cols-2 gap-3">
                    <input type="time" id="c-hora" class="p-4 bg-slate-50 rounded-xl outline-none font-bold">
                    <select id="c-pgto" class="p-4 bg-slate-50 rounded-xl outline-none font-bold"><option>Pix</option><option>Dinheiro</option><option>Cartão</option></select>
                </div>
                <button onclick="finalizarPedido()" id="btn-finish" class="w-full bg-green-500 text-white font-black py-5 rounded-2xl shadow-lg hover:bg-green-600 transition uppercase text-xs">Enviar Pedido via WhatsApp</button>
                <button onclick="closeCart()" class="w-full text-slate-400 font-bold py-2 text-xs uppercase">Fechar</button>
            </div>
        </div>
    </div>

    <script>
        const SENHA_ADM = "admin123";
        let tentativas = 0;
        let bloqueado = false;
        
        // Dados persistentes
        let ordens = JSON.parse(localStorage.getItem('ruan_os')) || [];
        let produtos = JSON.parse(localStorage.getItem('ruan_pr')) || [];
        let carrinho = [];
        let lang = 'pt';

        const dict = {
            pt: { client: "Acesso Cliente", admin: "Autenticar ADM", dash: "Dashboard", os: "Ordens", stock: "Mercado", logout: "SAIR", search: "Procurar...", cart: "Meu Carrinho", pick: "ADICIONAR", finish: "FINALIZAR" },
            en: { client: "Customer Access", admin: "Auth Admin", dash: "Stats", os: "Orders", stock: "Store", logout: "LOGOUT", search: "Search...", cart: "My Cart", pick: "ADD", finish: "CHECKOUT" },
            es: { client: "Acceso Cliente", admin: "Autenticar ADM", dash: "Tablero", os: "Órdenes", stock: "Mercado", logout: "SALIR", search: "Buscar...", cart: "Mi Carrito", pick: "AÑADIR", finish: "FINALIZAR" }
        };

        function toggleLangMenu() { document.getElementById('lang-menu').classList.toggle('hidden'); }

        function changeLang(l) {
            lang = l;
            document.getElementById('current-lang-name').innerText = (l === 'pt' ? 'Português' : l === 'en' ? 'English' : 'Español');
            document.getElementById('btn-client').innerText = dict[l].client;
            document.getElementById('btn-admin').innerText = dict[l].admin;
            document.getElementById('txt-cart-title').innerText = dict[l].cart;
            document.querySelectorAll('.txt-nav-dash').forEach(e => e.innerText = dict[l].dash);
            document.querySelectorAll('.txt-nav-os').forEach(e => e.innerText = dict[l].os);
            document.querySelectorAll('.txt-nav-stock').forEach(e => e.innerText = dict[l].stock);
            document.querySelectorAll('.txt-logout').forEach(e => e.innerText = dict[l].logout);
            document.getElementById('lang-menu').classList.add('hidden');
            renderAll();
        }

        function switchView(v) { 
            document.querySelectorAll('.view').forEach(e => e.classList.remove('active')); 
            document.getElementById(v + '-view').classList.add('active'); 
            renderAll();
        }

        function checkLogin() {
            if(bloqueado) return;
            const p = document.getElementById('login-pass').value;
            if(p === SENHA_ADM) {
                switchView('admin');
                tentativas = 0;
            } else {
                tentativas++;
                document.getElementById('login-error').classList.remove('hidden');
                document.getElementById('login-pass').value = '';
                if(tentativas >= 3) ativarLockdown();
            }
        }

        function ativarLockdown() {
            bloqueado = true;
            document.getElementById('lockdown-overlay').classList.remove('hidden');
            let t = 30;
            const timer = setInterval(() => {
                t--;
                document.getElementById('timer').innerText = t + "s";
                if(t <= 0) { clearInterval(timer); bloqueado = false; tentativas = 0; document.getElementById('lockdown-overlay').classList.add('hidden'); }
            }, 1000);
        }

        function switchTab(t) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active'));
            document.getElementById('tab-' + t).classList.add('active');
            document.getElementById('nav-' + t).classList.add('active');
        }

        function save() { 
            localStorage.setItem('ruan_os', JSON.stringify(ordens)); 
            localStorage.setItem('ruan_pr', JSON.stringify(produtos)); 
            renderAll(); 
        }

        function addOS() {
            const cliente = document.getElementById('os-cliente').value;
            if(!cliente) return;
            ordens.unshift({ id: Date.now(), cliente, whats: document.getElementById('os-whats').value, aparelho: document.getElementById('os-aparelho').value, status: 'Pendente' });
            document.getElementById('os-cliente').value = ''; document.getElementById('os-whats').value = ''; document.getElementById('os-aparelho').value = '';
            save();
        }

        function addProduto() {
            const nome = document.getElementById('p-nome').value;
            const qtd = parseInt(document.getElementById('p-qtd').value);
            const preco = parseFloat(document.getElementById('p-preco').value);
            if(!nome || isNaN(qtd)) return;
            produtos.push({ id: Date.now(), nome, qtd, preco });
            document.getElementById('p-nome').value = ''; document.getElementById('p-qtd').value = ''; document.getElementById('p-preco').value = '';
            save();
        }

        function renderAll() {
            const s = (document.getElementById('searchBar')?.value || "").toLowerCase();
            
            // Dashboard
            if(document.getElementById('dash-os')) {
                document.getElementById('dash-os').innerText = ordens.length;
                document.getElementById('dash-stock').innerText = produtos.length;
                document.getElementById('dash-alert').innerText = produtos.filter(p => p.qtd < 5).length;
            }

            // Tabela OS
            const osBody = document.getElementById('osTableBody');
            if(osBody) osBody.innerHTML = ordens.map((o,i) => `
                <tr class="hover:bg-slate-50">
                    <td class="p-6 font-bold text-slate-800">${o.cliente}</td>
                    <td class="p-6 font-medium">${o.aparelho}</td>
                    <td class="p-6"><span onclick="mStatus(${i})" class="cursor-pointer px-3 py-1 text-[10px] font-black rounded-full ${o.status==='Concluído'?'bg-green-100 text-green-700':'bg-amber-100 text-amber-700'}">${o.status}</span></td>
                    <td class="p-6 text-right"><button onclick="dOS(${i})" class="text-red-300"><i class="fas fa-trash"></i></button></td>
                </tr>`).join('');

            // Estoque
            const prBody = document.getElementById('prodTableBody');
            if(prBody) prBody.innerHTML = produtos.map((p,i) => `
                <tr>
                    <td class="p-6 font-bold">${p.nome}</td>
                    <td class="p-6 font-black">${p.qtd}</td>
                    <td class="p-6 font-bold text-green-600">R$ ${p.preco.toFixed(2)}</td>
                    <td class="p-6 text-right"><button onclick="dPr(${i})" class="text-red-300"><i class="fas fa-times-circle"></i></button></td>
                </tr>`).join('');

            // Loja
            const grid = document.getElementById('loja-grid');
            if(grid) grid.innerHTML = produtos.filter(p => p.nome.toLowerCase().includes(s)).map(p => `
                <div class="bg-white p-6 rounded-[2rem] shadow-sm border border-slate-50 flex flex-col justify-between">
                    <h3 class="font-black text-slate-800">${p.nome}</h3>
                    <div class="flex items-center justify-between mt-6">
                        <span class="text-xl font-black text-cyan-600 font-mono">R$ ${p.preco.toFixed(2)}</span>
                        <button onclick="addToCart(${p.id})" class="bg-slate-900 text-white px-4 py-2 rounded-xl text-[10px] font-black uppercase tracking-widest hover:bg-cyan-600 transition">${dict[lang].pick}</button>
                    </div>
                </div>`).join('');

            // Carrinho
            const cItems = document.getElementById('cart-items');
            if(cItems) {
                let t = 0;
                cItems.innerHTML = carrinho.map((c, i) => { 
                    t += c.preco * c.cQtd; 
                    return `<div class="bg-slate-50 p-4 rounded-xl flex justify-between items-center"><span 
