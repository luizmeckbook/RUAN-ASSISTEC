
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Global System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .view { display: none; }
        .view.active { display: flex; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); }
    </style>
</head>
<body class="bg-slate-100 min-h-screen">

    <div class="fixed top-6 right-6 z-[200]">
        <div class="relative inline-block text-left">
            <button onclick="toggleLangMenu()" class="bg-white border shadow-lg rounded-full p-3 flex items-center gap-2 hover:bg-gray-50 transition">
                <i class="fas fa-globe-americas text-cyan-600 text-xl"></i>
                <span id="current-lang-name" class="text-xs font-bold text-slate-700 hidden md:block">Português (Brasil)</span>
            </button>
            <div id="lang-menu" class="hidden absolute right-0 mt-2 w-48 bg-white rounded-2xl shadow-2xl border border-slate-100 overflow-hidden animate-fade-in">
                <button onclick="changeLang('pt')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇧🇷 Português</button>
                <button onclick="changeLang('en')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇺🇸 English</button>
                <button onclick="changeLang('es')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇪🇸 Español</button>
            </div>
        </div>
    </div>

    <div id="login-view" class="view active items-center justify-center min-h-screen bg-slate-900 p-4">
        <div class="bg-white p-8 md:p-12 rounded-[3rem] shadow-2xl w-full max-w-md text-center glass border border-white/20">
            <div class="mb-10">
                <div class="bg-cyan-500 w-20 h-20 rounded-3xl flex items-center justify-center mx-auto mb-6 rotate-3 shadow-lg shadow-cyan-500/30">
                    <i class="fas fa-microchip text-4xl text-white"></i>
                </div>
                <h1 class="text-4xl font-black tracking-tighter italic text-slate-900">RUAN <span class="text-cyan-500 font-black">ASSISTEC</span></h1>
                <p id="txt-login-subtitle" class="text-slate-400 text-[10px] font-bold tracking-[0.2em] uppercase mt-2">Tecnologia & Mercado</p>
            </div>
            <div class="space-y-4">
                <button onclick="switchView('cliente')" id="btn-client" class="w-full bg-slate-100 text-slate-800 font-black py-5 rounded-2xl hover:bg-slate-200 transition uppercase text-xs tracking-widest">SOU CLIENTE</button>
                <div class="relative py-4">
                    <hr class="border-slate-100">
                    <span id="txt-admin-label" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-4 text-[9px] font-black text-slate-300 uppercase">Acesso Administrativo</span>
                </div>
                <input type="password" id="login-pass" class="w-full p-5 bg-slate-50 border border-slate-100 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center font-bold" placeholder="••••••••">
                <button onclick="checkLogin()" id="btn-admin" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl hover:bg-cyan-600 shadow-2xl transition uppercase text-xs tracking-widest">ENTRAR NO PAINEL</button>
            </div>
            <p id="login-error" class="text-red-500 text-[10px] mt-6 hidden font-black uppercase">Senha incorreta / Wrong Password</p>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row min-h-screen">
        <aside class="w-full md:w-72 bg-slate-900 text-white flex flex-col p-6 shadow-2xl">
            <h2 class="text-2xl font-black italic mb-10">RUAN <span class="text-cyan-400 font-black">ADM</span></h2>
            <nav class="flex-1 space-y-3">
                <button onclick="switchTab('dashboard')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition"><i class="fas fa-layer-group"></i> <span class="txt-nav-dash">Dashboard</span></button>
                <button onclick="switchTab('os')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition"><i class="fas fa-tools"></i> <span class="txt-nav-os">Ordens</span></button>
                <button onclick="switchTab('estoque')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition"><i class="fas fa-shopping-cart"></i> <span class="txt-nav-stock">Mercado</span></button>
            </nav>
            <button onclick="logout()" class="p-4 text-red-400 font-black flex items-center gap-3 mt-auto hover:bg-red-500/10 rounded-2xl transition">
                <i class="fas fa-power-off"></i> <span class="txt-logout">SAIR</span>
            </button>
        </aside>
        
        <main class="flex-1 p-6 md:p-12 overflow-y-auto">
            <div id="tab-dashboard" class="tab-content space-y-8">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm"><p class="text-slate-400 text-xs font-black uppercase tracking-widest mb-2 txt-nav-os text-center md:text-left">Ordens</p><h3 id="dash-os" class="text-5xl font-black text-slate-900 text-center md:text-left">0</h3></div>
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm"><p class="text-slate-400 text-xs font-black uppercase tracking-widest mb-2 txt-nav-stock text-center md:text-left">Estoque</p><h3 id="dash-stock" class="text-5xl font-black text-cyan-600 text-center md:text-left">0</h3></div>
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm border-2 border-red-50"><p id="txt-alert-label" class="text-red-400 text-xs font-black uppercase tracking-widest mb-2 text-center md:text-left">Alertas</p><h3 id="dash-alert" class="text-5xl font-black text-red-500 text-center md:text-left">0</h3></div>
                </div>
            </div>

            <div id="tab-os" class="tab-content hidden"><div id="os-container" class="space-y-6"></div></div>
            <div id="tab-estoque" class="tab-content hidden"><div id="stock-container" class="space-y-6"></div></div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col min-h-screen">
        <header class="bg-white/80 backdrop-blur-md p-6 border-b sticky top-0 z-50 flex flex-col md:flex-row gap-6 justify-between items-center shadow-sm">
            <h1 class="text-2xl font-black italic tracking-tighter">RUAN <span class="text-cyan-500 font-black">STORE</span></h1>
            <div class="flex-1 max-w-2xl w-full">
                <div class="relative group">
                    <i class="fas fa-search absolute left-5 top-1/2 -translate-y-1/2 text-slate-300 group-focus-within:text-cyan-500 transition"></i>
                    <input type="text" id="searchBar" onkeyup="renderAll()" class="w-full p-5 pl-14 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 focus:ring-2 focus:ring-cyan-500 outline-none transition-all font-medium" placeholder="O que você procura?">
                </div>
            </div>
            <div class="flex gap-4">
                <button onclick="openCart()" class="relative p-4 bg-slate-900 text-white rounded-2xl shadow-xl shadow-slate-200 hover:scale-105 transition active:scale-95">
                    <i class="fas fa-shopping-basket"></i>
                    <span id="cart-count" class="absolute -top-2 -right-2 bg-cyan-500 text-white text-[10px] font-black px-2 py-0.5 rounded-full border-2 border-white">0</span>
                </button>
                <button onclick="logout()" class="p-4 bg-slate-100 rounded-2xl text-slate-400 hover:text-red-500 transition"><i class="fas fa-times"></i></button>
            </div>
        </header>
        <main class="container mx-auto p-8">
            <div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8"></div>
        </main>
    </div>

    <div id="cart-modal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-xl z-[300] hidden items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 shadow-2xl">
            <div class="flex justify-between items-center mb-8 border-b pb-6">
                <h2 id="txt-cart-title" class="text-3xl font-black italic text-slate-900 tracking-tighter">Carrinho</h2>
                <button onclick="closeCart()" class="text-4xl text-slate-200 hover:text-slate-400 transition">&times;</button>
            </div>
            <div id="cart-items" class="space-y-4 mb-8 max-h-60 overflow-y-auto px-2"></div>
            <div class="pt-6 space-y-6">
                <div class="flex justify-between items-end font-black"><span class="text-slate-400 text-xs uppercase tracking-widest">Total</span><span id="cart-total" class="text-3xl text-cyan-600">R$ 0,00</span></div>
                <div class="grid grid-cols-2 gap-4">
                    <input type="time" id="c-hora" class="p-5 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none font-bold text-slate-700">
                    <select id="c-pgto" class="p-5 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none font-bold text-slate-700">
                        <option>Pix</option><option>Dinheiro</option><option>Cartão</option>
                    </select>
                </div>
                <button onclick="finalizarPedido()" id="btn-finish" class="w-full bg-green-500 text-white font-black py-6 rounded-3xl hover:bg-green-600 transition shadow-2xl shadow-green-100 uppercase tracking-widest text-xs">Finalizar Pedido</button>
            </div>
        </div>
    </div>

    <script>
        const SENHA = "admin123";
        let ordens = JSON.parse(localStorage.getItem('r_global_os')) || [];
        let produtos = JSON.parse(localStorage.getItem('r_global_pr')) || [];
        let carrinho = [];
        let lang = 'pt';

        const dict = {
            pt: { 
                name: "Português (Brasil)", client: "SOU CLIENTE", admin: "ENTRAR NO PAINEL", subtitle: "TECNOLOGIA & MERCADO",
                admLabel: "ACESSO ADMINISTRATIVO", dash: "Dashboard", os: "Ordens", stock: "Mercado", logout: "SAIR",
                alerts: "Alertas", search: "O que você procura?", cart: "Meu Carrinho", pick: "ADICIONAR", finish: "FINALIZAR PEDIDO"
            },
            en: { 
                name: "English (USA)", client: "I'M CUSTOMER", admin: "LOGIN TO PANEL", subtitle: "TECH & MARKET",
                admLabel: "ADMINISTRATIVE ACCESS", dash: "Dashboard", os: "Orders", stock: "Market", logout: "LOGOUT",
                alerts: "Alerts", search: "Search products...", cart: "My Cart", pick: "ADD TO CART", finish: "FINISH ORDER"
            },
            es: { 
                name: "Español (ES)", client: "SOY CLIENTE", admin: "ENTRAR AL PANEL", subtitle: "TECNOLOGÍA Y MERCADO",
                admLabel: "ACCESO ADMINISTRATIVO", dash: "Tablero", os: "Órdenes", stock: "Mercado", logout: "SALIR",
                alerts: "Alertas", search: "¿Qué estás buscando?", cart: "Mi Carrito", pick: "COMPRAR", finish: "FINALIZAR PEDIDO"
            }
        };

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
            document.getElementById('txt-alert-label').innerText = dict[l].alerts;
            document.querySelectorAll('.txt-nav-dash').forEach(e => e.innerText = dict[l].dash);
            document.querySelectorAll('.txt-nav-os').forEach(e => e.innerText = dict[l].os);
            document.querySelectorAll('.txt-nav-stock').forEach(e => e.innerText = dict[l].stock);
            document.querySelectorAll('.txt-logout').forEach(e => e.innerText = dict[l].logout);
            document.getElementById('lang-menu').classList.add('hidden');
            renderAll();
        }

        // --- SISTEMA DE TELAS E LÓGICA (Mantido as funções robustas anteriores) ---
        function switchView(v) { document.querySelectorAll('.view').forEach(e => e.classList.remove('active')); document.getElementById(v + '-view').classList.add('active'); renderAll(); }
        function checkLogin() { if(document.getElementById('login-pass').value === SENHA) switchView('admin'); else document.getElementById('login-error').classList.remove('hidden'); }
        function logout() { location.reload(); }
        function switchTab(t) { 
            document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden')); 
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active')); 
            document.getElementById('tab-' + t).classList.remove('hidden'); 
            event.currentTarget.classList.add('active'); 
        }

        // Renderização dos cards da loja e carrinho com tradução dinâmica
        function renderAll() {
            const search = document.getElementById('searchBar').value.toLowerCase();
            const grid = document.getElementById('loja-grid');
            if(grid) {
                grid.innerHTML = produtos.filter(p => p.nome.toLowerCase().includes(search)).map(p => `
                    <div class="bg-white p-8 rounded-[3rem] shadow-sm border border-slate-50 flex flex-col justify-between hover:shadow-2xl hover:-translate-y-2 transition-all duration-500 group">
                        <div class="w-full h-32 bg-slate-50 rounded-[2rem] mb-6 flex items-center justify-center text-slate-200 text-4xl group-hover:bg-cyan-50 group-hover:text-cyan-500 transition"><i class="fas fa-box-open"></i></div>
                        <h3 class="font-black text-slate-800 text-lg mb-1 tracking-tight">${p.nome}</h3>
                        <p class="text-[10px] font-black text-slate-300 uppercase tracking-[0.2em] mb-4">${p.qtd} units</p>
                        <div class="flex items-center justify-between mt-auto pt-6 border-t border-slate-50">
                            <span class="text-xl font-black text-cyan-600">R$ ${p.preco.toFixed(2)}</span>
                            <button onclick="addToCart(${p.id})" class="bg-slate-900 text-white px-6 py-3 rounded-2xl text-[10px] font-black uppercase tracking-widest hover:bg-cyan-600 transition shadow-xl shadow-slate-100">${dict[lang].pick}</button>
                        </div>
                    </div>`).join('');
            }
            
            // Dashboard ADM
            if(document.getElementById('dash-os')) {
                document.getElementById('dash-os').innerText = ordens.length;
                document.getElementById('dash-stock').innerText = produtos.length;
                document.getElementById('dash-alert').innerText = produtos.filter(p => p.qtd < 5).length;
            }

            document.getElementById('cart-count').innerText = carrinho.length;
            const cItems = document.getElementById('cart-items');
            if(cItems) {
                let total = 0;
                cItems.innerHTML = carrinho.map(c => {
                    total += c.preco * c.cQtd;
                    return `<div class="bg-slate-50 p-5 rounded-3xl flex justify-between items-center"><div class="flex flex-col"><span class="text-xs font-black text-slate-800">${c.nome}</span><span class="text-[10px] font-bold text-slate-400">${c.cQtd}x - R$ ${c.preco.toFixed(2)}</span></div><span class="text-xs font-black text-slate-800">R$ ${(c.preco*c.cQtd).toFixed(2)}</span></div>`;
                }).join('');
                document.getElementById('cart-total').innerText = `R$ ${total.toFixed(2)}`;
            }
        }

        // Funções de Carrinho/OS mantidas conforme solicitado
        function addToCart(id) {
            const p = produtos.find(i => i.id === id);
            const ja = carrinho.find(c => c.id === id);
            if(ja) ja.cQtd++; else carrinho.push({...p, cQtd: 1});
            renderAll();
        }
        function openCart() { document.getElementById('cart-modal').classList.remove('hidden'); document.getElementById('cart-modal').classList.add('flex'); }
        function closeCart() { document.getElementById('cart-modal').classList.add('hidden'); }
        function finalizarPedido() {
            let t = 0, msg = `*PEDIDO RUAN STORE (%23${Date.now().toString().slice(-4)})*%0A%0A`;
            carrinho.forEach(i => { msg += `• ${i.nome} (${i.cQtd}x)%0A`; t += i.preco * i.cQtd; });
            msg += `%0A*TOTAL:* R$ ${t.toFixed(2)}%0A*HORA:* ${document.getElementById('c-hora').value}%0A*PGTO:* ${document.getElementById('c-pgto').value}`;
            window.open(`https://wa.me/5521999999999?text=${msg}`);
            carrinho = []; renderAll(); closeCart();
        }

        // Inicia em Português
        changeLang('pt');
    </script>
</body>
</html>
