<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Gestão Profissional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 min-h-screen">

    <header class="bg-slate-900 text-white p-5 shadow-xl border-b-4 border-cyan-500">
        <div class="container mx-auto flex justify-between items-center">
            <div>
                <h1 class="text-2xl font-black tracking-tighter">RUAN <span class="text-cyan-400">ASSISTEC</span></h1>
                <p class="text-xs text-slate-400 uppercase tracking-widest">Sistema de Gerenciamento</p>
            </div>
            <div class="text-right hidden md:block">
                <p id="data-atual" class="text-sm font-light text-slate-300"></p>
            </div>
        </div>
    </header>

    <main class="container mx-auto p-4 md:p-8">
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
            <div class="bg-white p-6 rounded-2xl shadow-sm border-l-4 border-cyan-500">
                <p class="text-slate-500 text-sm font-semibold uppercase">Total de Ordens</p>
                <h3 id="count-total" class="text-3xl font-bold text-slate-800">0</h3>
            </div>
            <div class="bg-white p-6 rounded-2xl shadow-sm border-l-4 border-yellow-500">
                <p class="text-slate-500 text-sm font-semibold uppercase">Em Aberto</p>
                <h3 id="count-aberto" class="text-3xl font-bold text-slate-800">0</h3>
            </div>
            <div class="bg-white p-6 rounded-2xl shadow-sm border-l-4 border-green-500">
                <p class="text-slate-500 text-sm font-semibold uppercase">Concluídas</p>
                <h3 id="count-concluido" class="text-3xl font-bold text-slate-800">0</h3>
            </div>
        </div>

        <section class="bg-white p-6 rounded-2xl shadow-lg mb-8 border border-slate-200">
            <h2 class="text-lg font-bold mb-5 text-slate-800 flex items-center">
                <i class="fas fa-plus-circle mr-2 text-cyan-500"></i> Nova Ordem de Serviço
            </h2>
            <form id="osForm" class="grid grid-cols-1 md:grid-cols-4 gap-4">
                <div class="col-span-1 md:col-span-1">
                    <label class="block text-xs font-bold text-slate-500 mb-1">CLIENTE</label>
                    <input type="text" id="cliente" placeholder="Nome completo" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-cyan-500 outline-none transition" required>
                </div>
                <div class="col-span-1 md:col-span-1">
                    <label class="block text-xs font-bold text-slate-500 mb-1">WHATSAPP (DDD)</label>
                    <input type="number" id="whatsapp" placeholder="Ex: 21999999999" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-cyan-500 outline-none transition" required>
                </div>
                <div class="col-span-1 md:col-span-1">
                    <label class="block text-xs font-bold text-slate-500 mb-1">EQUIPAMENTO</label>
                    <input type="text" id="aparelho" placeholder="Ex: Samsung S23" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-cyan-500 outline-none transition" required>
                </div>
                <div class="flex items-end">
                    <button type="submit" class="w-full bg-cyan-600 hover:bg-cyan-700 text-white font-bold py-3 rounded-xl shadow-lg shadow-cyan-200 transition duration-300 transform active:scale-95">
                        CADASTRAR OS
                    </button>
                </div>
            </form>
        </section>

        <section class="bg-white rounded-2xl shadow-lg border border-slate-200 overflow-hidden">
            <div class="p-5 border-b border-slate-100 bg-slate-50">
                <h2 class="text-lg font-bold text-slate-800">Serviços Ativos</h2>
            </div>
            <div class="overflow-x-auto">
                <table class="w-full text-left">
                    <thead class="bg-slate-50 text-slate-400 uppercase text-xs">
                        <tr>
                            <th class="p-5">ID</th>
                            <th class="p-5">Cliente / Whats</th>
                            <th class="p-5">Equipamento</th>
                            <th class="p-5 text-center">Status</th>
                            <th class="p-5 text-right">Ações</th>
                        </tr>
                    </thead>
                    <tbody id="osTableBody" class="divide-y divide-slate-100 text-slate-700">
                        </tbody>
                </table>
            </div>
        </section>
    </main>

    <script>
        const osForm = document.getElementById('osForm');
        const osTableBody = document.getElementById('osTableBody');
        
        // Data no Header
        document.getElementById('data-atual').innerText = new Date().toLocaleDateString('pt-br', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });

        // Banco de dados local
        let ordens = JSON.parse(localStorage.getItem('ruan_assistec_db')) || [];

        function atualizarDashboard() {
            document.getElementById('count-total').innerText = ordens.length;
            document.getElementById('count-aberto').innerText = ordens.filter(o => o.status === 'Pendente').length;
            document.getElementById('count-concluido').innerText = ordens.filter(o => o.status === 'Concluído').length;
        }

        function renderTable() {
            osTableBody.innerHTML = '';
            ordens.forEach((os, index) => {
                const statusColor = os.status === 'Concluído' ? 'bg-green-100 text-green-700' : 'bg-amber-100 text-amber-700';
                
                osTableBody.innerHTML += `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="p-5 font-mono text-sm text-slate-400">#${index + 100}</td>
                        <td class="p-5">
                            <p class="font-bold text-slate-800">${os.cliente}</p>
                            <p class="text-xs text-slate-500">${os.whatsapp}</p>
                        </td>
                        <td class="p-5 font-medium">${os.aparelho}</td>
                        <td class="p-5 text-center">
                            <span class="px-3 py-1 text-xs font-bold rounded-full ${statusColor}">${os.status}</span>
                        </td>
                        <td class="p-5 text-right space-x-2">
                            <button onclick="mudarStatus(${index})" class="text-cyan-600 hover:text-cyan-800 p-2" title="Alterar Status">
                                <i class="fas fa-sync-alt"></i>
                            </button>
                            <a href="https://wa.me/55${os.whatsapp}?text=Olá ${os.cliente}, aqui é da Ruan Assistec. Seu ${os.aparelho} está ${os.status.toLowerCase()}." target="_blank" class="text-green-500 hover:text-green-700 p-2">
                                <i class="fab fa-whatsapp"></i>
                            </a>
                            <button onclick="deletarOS(${index})" class="text-red-400 hover:text-red-600 p-2">
                                <i class="fas fa-trash"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            atualizarDashboard();
        }

        osForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const novaOS = {
                cliente: document.getElementById('cliente').value,
                whatsapp: document.getElementById('whatsapp').value,
                aparelho: document.getElementById('aparelho').value,
                status: 'Pendente'
            };
            ordens.unshift(novaOS); // Adiciona no início
            localStorage.setItem('ruan_assistec_db', JSON.stringify(ordens));
            osForm.reset();
            renderTable();
        });

        window.mudarStatus = (index) => {
            ordens[index].status = ordens[index].status === 'Pendente' ? 'Concluído' : 'Pendente';
            localStorage.setItem('ruan_assistec_db', JSON.stringify(ordens));
            renderTable();
        };

        window.deletarOS = (index) => {
            if(confirm('Deseja remover esta Ordem de Serviço?')) {
                ordens.splice(index, 1);
                localStorage.setItem('ruan_assistec_db', JSON.stringify(ordens));
                renderTable();
            }
        };

        // Início
        renderTable();
    </script>
</body>
</html>
