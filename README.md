
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Global System v5.1</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; overflow: hidden; }
        .view { display: none; width: 100%; min-height: 100vh; }
        .view.active { display: flex; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
    </style>
</head>
<body class="bg-slate-100">

    <div id="login-view" class="view active items-center justify-center bg-slate-900 p-4 relative">
        <div id="lockdown-overlay" class="hidden absolute inset-0 bg-black/95 z-[300] flex flex-col items-center justify-center text-white text-center">
            <i class="fas fa-shield-alt text-6xl text-red-500 mb-4 animate-bounce"></i>
            <h2 class="text-2xl font-black uppercase">Bloqueio de Segurança</h2>
            <p id="timer" class="text-4xl font-mono text-cyan-500 mt-4">30s</p>
        </div>

        <div class="bg-white p-8 md:p-12 rounded-[3rem] shadow-2xl w-full max-w-md text-center">
            <div class="mb-10">
                <div class="bg-cyan-500 w-20 h-20 rounded-3xl flex items-center justify-center mx-auto mb-4 shadow-lg">
                    <i class="fas fa-microchip text-4xl text-white"></i>
                </div>
                <h1 class="text-3xl font-black italic">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
            </div>
            
            <div class="space-y-4">
                <button onclick="switchView('cliente')" class="w-full bg-slate-100 text-slate-800 font-black py-5 rounded-2xl hover:bg-slate-200 transition uppercase text-xs tracking-widest">Acesso Cliente</button>
                
                <div class="relative py-4"><hr><span class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-4 text-[9px] font-black text-slate-300 uppercase">Administração</span></div>
                
                <input type="password" id="login-pass" class="w-full p-5 bg-slate-50 border border-slate-100 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center font-bold" placeholder="Senha ADM">
                <button onclick="checkLogin()" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl hover:bg-cyan-600 transition uppercase text-xs">Autenticar</button>
                <p id="login-error" class="text-red-500 text-[10px] hidden font-black uppercase mt-2">Senha Incorreta</p>
            </div>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row">
        <aside class="w-full md:w-72 bg-slate-900 text-white flex flex-col p-6">
            <h2 class="text-2xl font-black italic mb-10 text-cyan-400">RUAN ADM</h2>
            <nav class="flex-1 space-y-2">
                <button onclick="switchTab('dashboard')" id="nav-dashboard" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold active"><i class="fas fa-chart-pie"></i> Dashboard</button>
                <button onclick="switchTab('os')" id="nav-os" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold"><i class="fas fa-wrench"></i> Ordens</button>
                <button onclick="switchTab('estoque')" id="nav-estoque" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold"><i class="fas fa-store"></i> Mercado</button>
            </nav>
            <button onclick="location.reload()" class="p-4 text-red-400 font-black flex items-center gap-3 mt-auto hover:bg-red-500/10 rounded-xl transition">
                <i class="fas fa-power-off"></i> SAIR
            </button>
        </aside>

        <main class="flex-1 p-6 md:p-12 overflow-y-auto bg-slate-100">
            <div id="tab-dashboard" class="tab-content active space-y-6">
                <h2 class="text-2xl font-black text-slate-800 uppercase tracking-tighter">Visão Geral</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-l-8 border-cyan-500">
                        <p class="text-slate-400 text-xs font-black uppercase">O.S Ativas</p>
                        <h3 id="dash-os" class="text-4xl font-black">0</h3>
                    </div>
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-l-8 border-slate-800">
                        <p class="text-slate-400 text-xs font-black uppercase">Estoque</p>
                        <h3 id="dash-stock" class="text-4xl font-black">0</h3>
                    </div>
                </div>
            </div>

            <div id="tab-os" class="tab-content space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4">
                    <input type="text" id="os-cliente" placeholder="Cliente" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <button onclick="addOS()" class="bg-cyan-600 text-white font-black rounded-xl uppercase text-xs">Criar O.S</button>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-[10px] font-black uppercase text-slate-400">
                            <tr><th class="p-6">Cliente</th><th class="p-6">Aparelho</th><th class="p-6">Status</th><th class="p-6 text-right">Ação</th></tr>
                        </thead>
                        <tbody id="osTableBody" class="divide-y"></tbody>
                    </table>
                </div>
            </div>

            <div id="tab-estoque" class="tab-content space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4">
                    <input type="text" id="p-nome" placeholder="Produto" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <input type="number" id="p-qtd" placeholder="Qtd" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <input type="number" id="p-preco" placeholder="Preço" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <button onclick="addProduto()" class="bg-slate-900 text-white font-black rounded-xl uppercase text-xs">Salvar</button>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-[10px] font-black uppercase text-slate-400">
                            <tr><th class="p-6">Item</th><th class="p-6">Qtd</th><th class="p-6 text-right">Preço</th></tr>
                        </thead>
                        <tbody id="prodTableBody" class="divide-y"></tbody>
                    </table>
                </div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col overflow-y-auto">
        <header class="bg-white p-6 shadow-md flex justify-between items-center sticky top-0 z-50">
            <h1 class="text-2xl font-black italic">RUAN <span class="text-cyan-500">STORE</span></h1>
            <button onclick="location.reload()" class="p-4 bg-slate-100 rounded-2xl text-slate-500"><i class="fas fa-home"></i></button>
        </header>
        <main class="p-8 container mx-auto">
            <div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                </div>
        </main>
    </div>

    <script>
        const SENHA_ADM = "admin123";
        let tentativas = 0;
        let bloqueado = false;

        // Persistência
        let ordens = JSON.parse(localStorage.getItem('db_os')) || [];
        let produtos = JSON.parse(localStorage.getItem('db_pr')) || [];

        // NAVEGAÇÃO PRINCIPAL
        function switchView(viewId) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(viewId + '-view').classList.add('active');
            renderAll();
        }

        // LOGIN ADM
        function checkLogin() {
            if(bloqueado) return;
            const pass = document.getElementById('login-pass').value;
            if(pass === SENHA_ADM) {
                switchView('admin');
                tentativas = 0;
            } else {
                tentativas++;
                document.getElementById('login-error').classList.remove('hidden');
                if(tentativas >= 3) {
                    bloqueado = true;
                    document.getElementById('lockdown-overlay').classList.remove('hidden');
                    let t = 30;
                    const timer = setInterval(() => {
                        t--;
                        document.getElementById('timer').innerText = t + "s";
                        if(t <= 0) {
                            clearInterval(timer);
                            bloqueado = false;
                            tentativas = 0;
                            document.getElementById('lockdown-overlay').classList.add('hidden');
                        }
                    }, 1000);
                }
            }
        }

        // NAVEGAÇÃO ABAS ADM
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active'));
            document.getElementById('tab-' + tabId).classList.add('active');
            document.getElementById('nav-' + tabId).classList.add('active');
        }

        // FUNÇÕES DE DADOS
        function save() {
            localStorage.setItem('db_os', JSON.stringify(ordens));
            localStorage.setItem('db_pr', JSON.stringify(produtos));
            renderAll();
        }

        function addOS() {
            const c = document.getElementById('os-cliente').value;
            const a = document.getElementById('os-aparelho').value;
            if(!c || !a) return alert("Preencha os campos!");
            ordens.unshift({ id: Date.now(), cliente: c, aparelho: a, status: 'Pendente' });
            document.getElementById('os-cliente').value = '';
            document.getElementById('os-aparelho').value = '';
            save();
        }

        function addProduto() {
            const n = document.getElementById('p-nome').value;
            const q = document.getElementById('p-qtd').value;
            const p = document.getElementById('p-preco').value;
            if(!n || !q) return alert("Dados inválidos!");
            produtos.push({ id: Date.now(), nome: n, qtd: q, preco: parseFloat(p) });
            document.getElementById('p-nome').value = '';
            document.getElementById('p-qtd').value = '';
            document.getElementById('p-preco').value = '';
            save();
        }

        function renderAll() {
            // Dashboard
            if(document.getElementById('dash-os')) {
                document.getElementById('dash-os').innerText = ordens.length;
                document.getElementById('dash-stock').innerText = produtos.length;
            }

            // Tabela OS
            const osBody = document.getElementById('osTableBody');
            if(osBody) osBody.innerHTML = ordens.map((o, i) => `
                <tr>
                    <td class="p-6 font-bold text-slate-800">${o.cliente}</td>
                    <td class="p-6 font-medium">${o.aparelho}</td>
                    <td class="p-6"><span class="px-3 py-1 rounded-full text-[10px] font-black ${o.status === 'Pendente' ? 'bg-amber-100 text-amber-600' : 'bg-green-100 text-green-600'}">${o.status}</span></td>
                    <td class="p-6 text-right"><button onclick="ordens.splice(${i},1);save()" class="text-red-300 hover:text-red-600"><i class="fas fa-trash"></i></button></td>
                </tr>
            `).join('');

            // Tabela Estoque
            const prBody = document.getElementById('prodTableBody');
            if(prBody) prBody.innerHTML = produtos.map((p, i) => `
                <tr>
                    <td class="p-6 font-bold">${p.nome}</td>
                    <td class="p-6 font-black text-cyan-600">${p.qtd}</td>
                    <td class="p-6 text-right font-mono font-bold">R$ ${p.preco.toFixed(2)}</td>
                </tr>
            `).join('');

            // Loja Cliente
            const grid = document.getElementById('loja-grid');
            if(grid) grid.innerHTML = produtos.map(p => `
                <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100">
                    <h3 class="font-black text-slate-800 uppercase text-sm">${p.nome}</h3>
                    <p class="text-2xl font-black text-cyan-600 mt-4 font-mono">R$ ${p.preco.toFixed(2)}</p>
                    <button class="w-full mt-4 bg-slate-900 text-white py-3 rounded-xl font-black text-[10px] uppercase">Comprar</button>
                </div>
            `).join('');
        }

        renderAll();
    </script>
</body>
</html>
