
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Global System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .view { display: none; }
        .view.active { display: flex; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">

    <div id="login-view" class="view active items-center justify-center min-h-screen bg-slate-900 p-4">
        <div class="bg-white p-8 md:p-12 rounded-[2.5rem] shadow-2xl w-full max-w-md text-center">
            <div class="mb-8">
                <i class="fas fa-microchip text-6xl text-cyan-500 mb-4"></i>
                <h1 class="text-4xl font-black tracking-tighter italic">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
                <div class="flex justify-center gap-2 mt-4">
                    <button onclick="changeLang('pt')" title="Português">🇧🇷</button>
                    <button onclick="changeLang('en')" title="English">🇺🇸</button>
                    <button onclick="changeLang('es')" title="Español">🇪🇸</button>
                </div>
            </div>
            <div class="space-y-4">
                <button onclick="switchView('cliente')" id="btn-client" class="w-full bg-gray-100 text-gray-700 font-bold py-4 rounded-2xl hover:bg-gray-200 transition uppercase tracking-tighter">Entrar como Cliente</button>
                <div class="relative py-2"><hr><span class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-3 text-[10px] font-bold text-gray-400">ADMINISTRATIVO</span></div>
                <input type="password" id="login-pass" class="w-full p-4 bg-gray-50 border border-gray-200 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center" placeholder="••••••••">
                <button onclick="checkLogin()" id="btn-admin" class="w-full bg-slate-900 text-white font-bold py-4 rounded-2xl hover:bg-cyan-600 shadow-xl transition">ACESSAR PAINEL</button>
            </div>
            <p id="login-error" class="text-red-500 text-xs mt-4 hidden font-bold">SENHA INCORRETA!</p>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row min-h-screen">
        <aside class="w-full md:w-64 bg-slate-900 text-white flex flex-col p-4">
            <h2 class="text-2xl font-black p-4 italic">RUAN <span class="text-cyan-400">ADM</span></h2>
            <nav class="flex-1 space-y-2 mt-4">
                <button onclick="switchTab('dashboard')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-3 font-bold"><i class="fas fa-home"></i> Dashboard</button>
                <button onclick="switchTab('os')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-3 font-bold"><i class="fas fa-wrench"></i> Ordens</button>
                <button onclick="switchTab('estoque')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-3 font-bold"><i class="fas fa-store"></i> Mercado</button>
            </nav>
            <button onclick="logout()" class="p-4 text-red-400 font-bold flex items-center gap-2"><i class="fas fa-power-off"></i> Sair</button>
        </aside>
        
        <main class="flex-1 p-6 md:p-10 overflow-y-auto">
            <div id="tab-dashboard" class="tab-content space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="bg-white p-8 rounded-[2rem] shadow-sm"><p class="text-gray-400 text-xs font-bold uppercase">Ordens</p><h3 id="dash-os" class="text-4xl font-black text-slate-800">0</h3></div>
                    <div class="bg-white p-8 rounded-[2rem] shadow-sm"><p class="text-gray-400 text-xs font-bold uppercase">Estoque</p><h3 id="dash-stock" class="text-4xl font-black text-cyan-600">0</h3></div>
                    <div class="bg-white p-8 rounded-[2rem] shadow-sm border-2 border-red-50"><p class="text-red-400 text-xs font-bold uppercase">Alertas</p><h3 id="dash-alert" class="text-4xl font-black text-red-500">0</h3></div>
                </div>
            </div>

            <div id="tab-os" class="tab-content hidden">
                <form id="osForm" class="bg-white p-6 rounded-[2rem] shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
                    <input type="text" id="os-cliente" placeholder="Nome Cliente" class="p-4 bg-gray-50 rounded-2xl border outline-none">
                    <input type="number" id="os-whats" placeholder="WhatsApp" class="p-4 bg-gray-50 rounded-2xl border outline-none">
                    <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-4 bg-gray-50 rounded-2xl border outline-none">
                    <button class="bg-cyan-600 text-white font-black rounded-2xl uppercase">Criar OS</button>
                </form>
                <div class="bg-white rounded-[2rem] shadow-sm overflow-hidden">
                    <table class="w-full text-left border-collapse">
                        <thead class="bg-gray-50 border-b text-[10px] uppercase text-gray-400 font-black">
                            <tr><th class="p-5">ID</th><th class="p-5">Cliente</th><th class="p-5">Aparelho</th><th class="p-5">Status</th><th class="p-5 text-right">Ações</th></tr>
                        </thead>
                        <tbody id="osTableBody" class="divide-y divide-gray-50"></tbody>
                    </table>
                </div>
            </div>

            <div id="tab-estoque" class="tab-content hidden">
                <form id="prodForm" class="bg-white p-6 rounded-[2rem] shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
                    <input type="text" id="p-nome" placeholder="Produto" class="p-4 bg-gray-50 rounded-2xl border outline-none">
                    <input type="number" id="p-qtd" placeholder="Qtd" class="p-4 bg-gray-50 rounded-2xl border outline-none">
                    <input type="number" step="0.01" id="p-preco" placeholder="Preço" class="p-4 bg-gray-50 rounded-2xl border outline-none">
                    <button class="bg-slate-900 text-white font-black rounded-2xl uppercase">Adicionar</button>
                </form>
                <div class="bg-white rounded-[2rem] shadow-sm overflow-hidden">
                    <table class="w-full text-left border-collapse">
                        <thead class="bg-gray-50 border-b text-[10px] uppercase text-gray-400 font-black">
                            <tr><th class="p-5">Item</th><th class="p-5 text-center">Qtd</th><th class="p-5">Preço</th><th class="p-5 text-right">Ações</th></tr>
                        </thead>
                        <tbody id="prodTableBody" class="divide-y divide-gray-50"></tbody>
                    </table>
                </div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col min-h-screen">
        <header class="bg-white p-6 shadow-sm border-b sticky top-0 z-50 flex flex-col md:flex-row gap-4 justify-between items-center">
            <h1 class="text-2xl font-black italic">RUAN <span class="text-cyan-500">STORE</span></h1>
            <div class="flex-1 max-w-xl w-full">
                <div class="relative">
                    <i class="fas fa-search absolute left-4 top-1/2 -translate-y-1/2 text-gray-300"></i>
                    <input type="text" id="searchBar" onkeyup="renderAll()" class="w-full p-4 pl-12 bg-gray-50 rounded-2xl border-none ring-1 ring-gray-100 focus:ring-2 focus:ring-cyan-500 outline-none" placeholder="O que você procura?">
                </div>
            </div>
            <div class="flex gap-4">
                <button onclick="openCart()" class="relative p-3 bg-slate-900 text-white rounded-2xl"><i class="fas fa-shopping-basket"></i><span id="cart-count" class="absolute -top-2 -right-2 bg-cyan-500 text-[10px] px-2 py-0.5 rounded-full">0</span></button>
                <button onclick="logout()" class="p-3 bg-gray-100 rounded-2xl text-gray-400"><i class="fas fa-times"></i></button>
            </div>
        </header>
        <main class="container mx-auto p-6">
            <div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-6"></div>
        </main>
    </div>

    <div id="cart-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[100] hidden items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 shadow-2xl">
            <div class="flex justify-between items-center mb-6">
                <h2 id="txt-cart-title" class="text-2xl font-black italic">Carrinho</h2>
                <button onclick="closeCart()" class="text-3xl text-gray-300">&times;</button>
            </div>
            <div id="cart-items" class="space-y-4 mb-6 max-h-48 overflow-y-auto px-2"></div>
            <div class="border-t pt-6 space-y-4">
                <div class="flex justify-between font-black text-xl"><span>TOTAL</span><span id="cart-total" class="text-cyan-600">R$ 0,00</span></div>
                <div class="grid grid-cols-2 gap-4">
                    <input type="time" id="c-hora" class="p-4 bg-gray-50 rounded-2xl border text-sm">
                    <select id="c-pgto" class="p-4 bg-gray-50 rounded-2xl border text-sm">
                        <option>Pix</option><option>Dinheiro</option><option>Cartão</option>
                    </select>
                </div>
                <button onclick="finalizarPedido()" class="w-full bg-green-500 text-white font-black py-5 rounded-[1.5rem] hover:bg-green-600 transition shadow-xl shadow-green-100 uppercase">Finalizar Pedido</button>
            </div>
        </div>
    </div>

    <script>
        const SENHA = "admin123";
        let ordens = JSON.parse(localStorage.getItem('r_os_v2')) || [];
        let produtos = JSON.parse(localStorage.getItem('r_pr_v2')) || [];
        let carrinho = [];
        let lang = 'pt';

        const dict = {
            pt: { client: "SOU CLIENTE", admin: "ENTRAR NO PAINEL", cart: "CARRINHO", search: "O que você procura?", pick: "PEGAR" },
            en: { client: "I AM CUSTOMER", admin: "ACCESS PANEL", cart: "SHOPPING CART", search: "What are you looking for?", pick: "GET" },
            es: { client: "SOY CLIENTE", admin: "ACCEDER PANEL", cart: "CARRITO", search: "¿Qué estás buscando?", pick: "COMPRAR" }
        };

        function changeLang(l) {
            lang = l;
            document.getElementById('btn-client').innerText = dict[l].client;
            document.getElementById('btn-admin').innerText = dict[l].admin;
            document.getElementById('searchBar').placeholder = dict[l].search;
            document.getElementById('txt-cart-title').innerText = dict[l].cart;
            renderAll();
        }

        function switchView(v) { 
            document.querySelectorAll('.view').forEach(e => e.classList.remove('active'));
            document.getElementById(v + '-view').classList.add('active');
            renderAll();
        }

        function checkLogin() {
            if(document.getElementById('login-pass').value === SENHA) switchView('admin');
            else { document.getElementById('login-error').classList.remove('hidden'); }
        }

        function logout() { location.reload(); }

        function switchTab(t) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active'));
            document.getElementById('tab-' + t).classList.remove('hidden');
            event.currentTarget.classList.add('active');
        }

        document.getElementById('osForm').addEventListener('submit', (e) => {
            e.preventDefault();
            ordens.unshift({ id: Math.floor(Math.random()*9000)+1000, cliente: document.getElementById('os-cliente').value, whatsapp: document.getElementById('os-whats').value, aparelho: document.getElementById('os-aparelho').value, status: 'Pendente' });
            save(); e.target.reset();
        });

        document.getElementById('prodForm').addEventListener('submit', (e) => {
            e.preventDefault();
            produtos.push({ id: Date.now(), nome: document.getElementById('p-nome').value, qtd: parseInt(document.getElementById('p-qtd').value), preco: parseFloat(document.getElementById('p-preco').value) });
            save(); e.target.reset();
        });

        function addToCart(id) {
            const p = produtos.find(i => i.id === id);
            const ja = carrinho.find(c => c.id === id);
            if(ja) ja.cQtd++; else carrinho.push({...p, cQtd: 1});
            renderAll();
        }

        function openCart() { document.getElementById('cart-modal').classList.remove('hidden'); document.getElementById('cart-modal').classList.add('flex'); }
        function closeCart() { document.getElementById('cart-modal').classList.add('hidden'); }

        function finalizarPedido() {
            let total = 0, msg = `*PEDIDO RUAN STORE*%0A%0A`;
            carrinho.forEach(i => { msg += `• ${i.nome} (${i.cQtd}x)%0A`; total += i.preco * i.cQtd; });
            msg += `%0A*TOTAL:* R$ ${total.toFixed(2)}%0A*HORA:* ${document.getElementById('c-hora').value}`;
            window.open(`https://wa.me/5521999999999?text=${msg}`);
            carrinho = []; renderAll(); closeCart();
        }

        function save() { 
            localStorage.setItem('r_os_v2', JSON.stringify(ordens)); 
            localStorage.setItem('r_pr_v2', JSON.stringify(produtos)); 
            renderAll(); 
        }

        function renderAll() {
            const search = document.getElementById('searchBar').value.toLowerCase();
            
            // Dashboard
            if(document.getElementById('dash-os')) {
                document.getElementById('dash-os').innerText = ordens.length;
                document.getElementById('dash-stock').innerText = produtos.length;
                document.getElementById('dash-alert').innerText = produtos.filter(p => p.qtd < 5).length;
            }

            // OS e Estoque ADM
            const osBody = document.getElementById('osTableBody');
            if(osBody) osBody.innerHTML = ordens.map((o,i) => `<tr class="text-sm"><td class="p-5 text-gray-400">#${o.id}</td><td class="p-5 font-bold">${o.cliente}</td><td class="p-5 font-medium">${o.aparelho}</td><td class="p-5"><span class="px-3 py-1 text-[10px] font-black rounded-full ${o.status==='Concluído'?'bg-green-100 text-green-700':'bg-amber-100 text-amber-700'}">${o.status}</span></td><td class="p-5 text-right"><button onclick="mStatus(${i})" class="text-cyan-500 mr-2"><i class="fas fa-check-circle"></i></button><button onclick="dOS(${i})" class="text-red-200"><i class="fas fa-trash"></i></button></td></tr>`).join('');
            
            const prBody = document.getElementById('prodTableBody');
            if(prBody) prBody.innerHTML = produtos.map((p,i) => `<tr class="text-sm"><td class="p-5 font-bold">${p.nome}</td><td class="p-5 text-center font-black">${p.qtd}</td><td class="p-5 font-bold text-green-600">R$ ${p.preco.toFixed(2)}</td><td class="p-5 text-right"><button onclick="dPr(${i})" class="text-red-400 px-4"><i class="fas fa-times-circle"></i></button></td></tr>`).join('');

            // Loja Cliente com Busca
            const grid = document.getElementById('loja-grid');
            if(grid) grid.innerHTML = produtos.filter(p => p.nome.toLowerCase().includes(search)).map(p => `
                <div class="bg-white p-6 rounded-[2.5rem] shadow-sm border border-slate-50 flex flex-col justify-between hover:shadow-2xl transition-all duration-500 group">
                    <div class="w-full h-24 bg-slate-50 rounded-[1.5rem] mb-4 flex items-center justify-center text-slate-200 text-3xl group-hover:bg-cyan-50 group-hover:text-cyan-200 transition"><i class="fas fa-box"></i></div>
                    <h3 class="font-bold text-slate-800">${p.nome}</h3>
                    <div class="mt-4 flex items-center justify-between font-black">
                        <span class="text-cyan-600 text-lg">R$ ${p.preco.toFixed(2)}</span>
                        <button onclick="addToCart(${p.id})" class="bg-slate-900 text-white px-5 py-2.5 rounded-2xl text-[10px] uppercase tracking-widest hover:bg-cyan-600 transition">${dict[lang].pick}</button>
                    </div>
                </div>`).join('');

            document.getElementById('cart-count').innerText = carrinho.length;
            const cItems = document.getElementById('cart-items');
            if(cItems) {
                let t = 0;
                cItems.innerHTML = carrinho.map(c => { t += c.preco*c.cQtd; return `<div class="bg-slate-50 p-4 rounded-2xl flex justify-between items-center"><span class="text-xs font-bold">${c.nome} (${c.cQtd}x)</span><span class="text-xs font-mono">R$ ${(c.preco*c.cQtd).toFixed(2)}</span></div>`}).join('');
                document.getElementById('cart-total').innerText = `R$ ${t.toFixed(2)}`;
            }
        }

        window.mStatus = (i) => { ordens[i].status = ordens[i].status === 'Pendente' ? 'Concluído' : 'Pendente'; save(); };
        window.dOS = (i) => { if(confirm('Excluir?')) { ordens.splice(i,1); save(); } };
        window.dPr = (i) => { if(confirm('Remover?')) { produtos.splice(i,1); save(); } };

        renderAll();
    </script>
</body>
</html>
