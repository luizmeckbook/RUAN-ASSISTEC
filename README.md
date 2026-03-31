<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Medela Supermercado - Sistema Corrigido</title>
    <style>
        :root { --primary: #e53935; --admin-bg: #2c3e50; --bg: #f4f4f4; --whatsapp: #25d366; }
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; -webkit-tap-highlight-color: transparent; }
        body { margin: 0; background: var(--bg); color: #333; overflow-x: hidden; }
        
        /* Sistema de Telas */
        .tela { display: none; min-height: 100vh; width: 100%; position: absolute; top: 0; left: 0; }
        .ativa { display: block !important; position: relative; }
        
        .container { max-width: 500px; margin: auto; padding: 15px; }
        .card { background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); margin-bottom: 15px; }
        
        /* Inputs e Botões */
        input, select, textarea { width: 100%; padding: 14px; margin: 10px 0; border: 1px solid #ccc; border-radius: 10px; font-size: 16px; outline: none; }
        button { width: 100%; padding: 14px; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; font-size: 16px; transition: 0.2s; margin-top: 5px; color: white; }
        button:active { transform: scale(0.96); opacity: 0.8; }
        
        .btn-main { background: var(--primary); }
        .btn-sec { background: var(--admin-bg); }
        .btn-whats { background: var(--whatsapp); display: flex; align-items: center; justify-content: center; gap: 10px; }
        
        /* Header */
        .header { background: var(--primary); color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; height: 60px; }
        
        /* Grid e Outros */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .prod-card { background: white; padding: 15px; border-radius: 12px; text-align: center; border: 1px solid #eee; }
        .cart-float { position: fixed; bottom: 85px; right: 20px; width: 60px; height: 60px; background: #000; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 24px; cursor: pointer; box-shadow: 0 4px 15px rgba(0,0,0,0.4); z-index: 99; }
        .badge { position: absolute; top: 0; right: 0; background: var(--primary); width: 22px; height: 22px; border-radius: 50%; font-size: 12px; display: flex; align-items: center; justify-content: center; }
    </style>
</head>
<body>

<div id="tela_login" class="tela ativa">
    <div class="container" style="padding-top: 50px; text-align: center;">
        <h1 id="txt_loja_nome">Medela Supermercado</h1>
        <div class="card">
            <h3>Entrar</h3>
            <input type="text" id="login_cpf" placeholder="CPF ou 'admin'">
            <input type="password" id="login_senha" placeholder="Senha">
            <button type="button" class="btn-main" onclick="funcaoLogin(event)">ACESSAR PAINEL</button>
            <button type="button" class="btn-whats" onclick="abrirSuporte(event)">💬 SUPORTE CHAT</button>
            <p onclick="irPara('tela_cadastro')" style="margin-top:20px; color:var(--primary); cursor:pointer; font-weight:bold">Não tem conta? Cadastre-se</p>
        </div>
    </div>
</div>

<div id="tela_cadastro" class="tela">
    <div class="container">
        <h2>Criar Conta</h2>
        <div class="card">
            <input id="cad_nome" placeholder="Nome Completo">
            <input id="cad_cpf" placeholder="CPF (apenas números)">
            <input id="cad_senha" type="password" placeholder="Senha">
            <button type="button" class="btn-main" onclick="funcaoCadastro(event)">FINALIZAR CADASTRO</button>
            <button type="button" class="btn-sec" onclick="irPara('tela_login')">VOLTAR AO LOGIN</button>
        </div>
    </div>
</div>

<div id="tela_home" class="tela">
    <div class="header">
        <span id="txt_boas_vindas">Olá!</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(0,0,0,0.2); padding:8px 15px">Sair</button>
    </div>
    <div class="container">
        <button type="button" class="btn-sec" onclick="irPara('tela_rastreio')" style="margin-bottom:15px">📦 MEUS PEDIDOS / RASTREIO</button>
        <div class="grid" id="lista_produtos"></div>
    </div>
    <div class="cart-float" onclick="irPara('tela_carrinho')">
        🛒 <span class="badge" id="cart_count">0</span>
    </div>
    <button type="button" onclick="abrirSuporte(event)" style="position:fixed; bottom:20px; right:20px; width:60px; height:60px; border-radius:50%; background:var(--whatsapp); font-size:25px; box-shadow: 0 4px 10px rgba(0,0,0,0.3); z-index:99;">💬</button>
</div>

<div id="tela_carrinho" class="tela">
    <div class="header">
        <button type="button" onclick="irPara('tela_home')" style="width:auto; background:none; font-size:20px">←</button>
        <span>Meu Carrinho</span>
        <div style="width:40px"></div>
    </div>
    <div class="container">
        <div id="itens_no_carrinho"></div>
        <div class="card" id="checkout_box" style="display:none">
            <select id="sel_entrega" onchange="atualizarTotal()">
                <option value="retirada">Retirar na Loja (Grátis)</option>
                <option value="entrega">Entrega em Casa (+ R$ 7,00)</option>
            </select>
            <textarea id="endereco_entrega" placeholder="Endereço: Rua, Nº, Bairro" style="display:none"></textarea>
            <select id="sel_pgto">
                <option value="">Forma de Pagamento</option>
                <option value="Pix">Pix</option>
                <option value="Cartão">Cartão</option>
                <option value="Dinheiro">Dinheiro</option>
            </select>
            <h2 id="txt_total" style="text-align:right; color: green;">Total: R$ 0,00</h2>
            <button type="button" class="btn-whats" onclick="gerarPedido(event)">🚀 FINALIZAR NO WHATSAPP</button>
        </div>
    </div>
</div>

<div id="tela_rastreio" class="tela">
    <div class="header">
        <button type="button" onclick="irPara('tela_home')" style="width:auto; background:none; font-size:20px">←</button>
        <span>Rastreamento</span>
        <div style="width:40px"></div>
    </div>
    <div class="container" id="lista_meus_pedidos"></div>
</div>

<div id="tela_admin" class="tela">
    <div class="header" style="background:var(--admin-bg)">
        <span>PAINEL GESTOR</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(255,255,255,0.1)">Sair</button>
    </div>
    <div class="container">
        <div class="card" style="background:#2ecc71; color:white">
            <small>Faturamento Total</small>
            <h2 id="adm_faturamento">R$ 0,00</h2>
        </div>
        <div class="card">
            <h3>Configurações</h3>
            <input id="adm_cfg_nome" placeholder="Nome da Loja">
            <input id="adm_cfg_whats" placeholder="WhatsApp (DDI+DDD+N)">
            <button type="button" class="btn-sec" onclick="admSalvarConfig(event)">SALVAR ALTERAÇÕES</button>
        </div>
        <div class="card">
            <h3>Cadastrar Produto</h3>
            <input id="adm_p_nome" placeholder="Nome do Produto">
            <input id="adm_p_preco" type="number" step="0.01" placeholder="Preço de Venda">
            <button type="button" class="btn-main" onclick="admAddProduto(event)">CADASTRAR</button>
        </div>
        <div class="card">
            <h3>Pedidos e Suporte</h3>
            <div id="adm_lista_pedidos"></div>
        </div>
    </div>
</div>

<div id="tela_chat" class="tela">
    <div class="header" style="background:#075e54">
        <button type="button" onclick="voltarDoChat()" style="width:auto; background:none; font-size:20px">←</button>
        <span>Suporte Online</span>
        <div style="width:40px"></div>
    </div>
    <div class="container">
        <div id="chat_mensagens" style="height:350px; overflow-y:auto; background:#e5ddd5; padding:15px; border-radius:10px; display:flex; flex-direction:column; gap:10px;"></div>
        <div style="display:flex; gap:5px; margin-top:10px">
            <input id="chat_input" placeholder="Sua mensagem...">
            <button type="button" onclick="chatEnviar()" style="width:60px; margin:0" class="btn-whats">➤</button>
        </div>
    </div>
</div>

<script>
// --- BANCO DE DADOS LOCAL ---
let db = {
    users: JSON.parse(localStorage.getItem("db_users")) || {},
    prods: JSON.parse(localStorage.getItem("db_prods")) || [],
    pedidos: JSON.parse(localStorage.getItem("db_pedidos")) || [],
    config: JSON.parse(localStorage.getItem("db_config")) || { nome: "Medela Supermercado", whats: "" },
    chat: JSON.parse(localStorage.getItem("db_chat")) || {}
};

function salvar() {
    localStorage.setItem("db_users", JSON.stringify(db.users));
    localStorage.setItem("db_prods", JSON.stringify(db.prods));
    localStorage.setItem("db_pedidos", JSON.stringify(db.pedidos));
    localStorage.setItem("db_config", JSON.stringify(db.config));
    localStorage.setItem("db_chat", JSON.stringify(db.chat));
}

let sessao = "";
let carrinho = [];

// --- SISTEMA DE NAVEGAÇÃO ---
function irPara(telaId) {
    document.querySelectorAll('.tela').forEach(t => t.classList.remove('ativa'));
    const destino = document.getElementById(telaId);
    if(destino) {
        destino.classList.add('ativa');
        if(telaId === 'tela_home') rendHome();
        if(telaId === 'tela_carrinho') rendCarrinho();
        if(telaId === 'tela_rastreio') rendRastreio();
        if(telaId === 'tela_admin') rendAdmin();
        window.scrollTo(0,0);
    }
}

// --- LOGIN E CADASTRO ---
function funcaoLogin(e) {
    if(e) e.preventDefault();
    const c = document.getElementById("login_cpf").value.trim();
    const s = document.getElementById("login_senha").value.trim();

    if(c === 'admin' && s === 'admin123') {
        sessao = "admin";
        irPara('tela_admin');
    } else if(db.users[c] && db.users[c].senha === s) {
        sessao = c;
        irPara('tela_home');
    } else {
        alert("Dados incorretos!");
    }
}

function funcaoCadastro(e) {
    if(e) e.preventDefault();
    const n = document.getElementById("cad_nome").value;
    const c = document.getElementById("cad_cpf").value;
    const s = document.getElementById("cad_senha").value;
    if(!n || !c || !s) return alert("Preencha tudo!");
    db.users[c] = { nome: n, senha: s };
    salvar();
    alert("Cadastrado com sucesso!");
    irPara('tela_login');
}

function logout() {
    sessao = "";
    carrinho = [];
    document.getElementById("cart_count").innerText = "0";
    irPara('tela_login');
}

// --- LOJA E CARRINHO ---
function rendHome() {
    document.getElementById("txt_boas_vindas").innerText = "Olá, " + (db.users[sessao]?.nome || "Cliente");
    const lista = document.getElementById("lista_produtos");
    lista.innerHTML = db.prods.map((p, i) => `
        <div class="prod-card">
            <strong>${p.nome}</strong><br>
            <span style="color:green; font-weight:bold">R$ ${p.preco.toFixed(2)}</span>
            <button type="button" class="btn-main" onclick="addCarrinho(${i})">ADICIONAR</button>
        </div>
    `).join('') || "<p>Nenhum produto disponível.</p>";
}

function addCarrinho(i) {
    carrinho.push(db.prods[i]);
    document.getElementById("cart_count").innerText = carrinho.length;
}

function rendCarrinho() {
    const box = document.getElementById("itens_no_carrinho");
    if(carrinho.length === 0) {
        box.innerHTML = "<p style='text-align:center'>Vazio.</p>";
        document.getElementById("checkout_box").style.display = "none";
        return;
    }
    document.getElementById("checkout_box").style.display = "block";
    box.innerHTML = carrinho.map((it, idx) => `
        <div class="card" style="display:flex; justify-content:space-between; padding:10px">
            <span>${it.nome}</span>
            <b>R$ ${it.preco.toFixed(2)}</b>
        </div>
    `).join('');
    atualizarTotal();
}

function atualizarTotal() {
    let t = carrinho.reduce((a, b) => a + b.preco, 0);
    const ent = document.getElementById("sel_entrega").value;
    document.getElementById("endereco_entrega").style.display = ent === 'entrega' ? 'block' : 'none';
    if(ent === 'entrega') t += 7;
    document.getElementById("txt_total").innerText = "Total: R$ " + t.toFixed(2);
}

function gerarPedido(e) {
    if(e) e.preventDefault();
    const ent = document.getElementById("sel_entrega").value;
    const pgto = document.getElementById("sel_pgto").value;
    if(!pgto) return alert("Escolha o pagamento!");

    let total = carrinho.reduce((a, b) => a + b.preco, 0) + (ent === 'entrega' ? 7 : 0);
    const novo = { id: "#"+Math.floor(Math.random()*9000), user: sessao, total: total, status: "Pendente", itens: carrinho.map(it => it.nome).join(", ") };
    
    db.pedidos.unshift(novo);
    salvar();
    
    const msg = `Olá! Pedido ${novo.id}\nItens: ${novo.itens}\nTotal: R$ ${total.toFixed(2)}`;
    window.open(`https://api.whatsapp.com/send?phone=${db.config.whats}&text=${encodeURIComponent(msg)}`);
    
    carrinho = [];
    document.getElementById("cart_count").innerText = "0";
    irPara('tela_rastreio');
}

function rendRastreio() {
    const meus = db.pedidos.filter(p => p.user === sessao);
    document.getElementById("lista_meus_pedidos").innerHTML = meus.map(p => `
        <div class="card">
            <b>Pedido ${p.id}</b> - <span style="color:red">${p.status}</span><br>
            <small>${p.itens}</small><br>
            <strong>R$ ${p.total.toFixed(2)}</strong>
        </div>
    `).join('') || "<p>Sem pedidos.</p>";
}

// --- ADMIN ---
function rendAdmin() {
    const faturamento = db.pedidos.reduce((a, b) => a + b.total, 0);
    document.getElementById("adm_faturamento").innerText = "R$ " + faturamento.toFixed(2);
    document.getElementById("adm_cfg_nome").value = db.config.nome;
    document.getElementById("adm_cfg_whats").value = db.config.whats;

    document.getElementById("adm_lista_pedidos").innerHTML = db.pedidos.map(p => `
        <div style="border-bottom:1px solid #eee; padding:10px 0">
            <b>${p.id}</b> - R$ ${p.total.toFixed(2)}<br>
            <small>${p.itens}</small>
        </div>
    `).join('') || "Sem pedidos.";
}

function admAddProduto(e) {
    if(e) e.preventDefault();
    const n = document.getElementById("adm_p_nome").value;
    const p = parseFloat(document.getElementById("adm_p_preco").value);
    if(!n || !p) return;
    db.prods.push({ nome: n, preco: p });
    salvar();
    alert("Produto salvo!");
    rendAdmin();
}

function admSalvarConfig(e) {
    if(e) e.preventDefault();
    db.config.nome = document.getElementById("adm_cfg_nome").value;
    db.config.whats = document.getElementById("adm_cfg_whats").value;
    salvar();
    document.getElementById("txt_loja_nome").innerText = db.config.nome;
    alert("Configurações salvas!");
}

// --- CHAT ---
function abrirSuporte(e) {
    if(e) e.preventDefault();
    if(!sessao) sessao = "visitante_" + Math.floor(Math.random()*100);
    irPara('tela_chat');
    rendChat();
}

function chatEnviar() {
    const txt = document.getElementById("chat_input").value;
    if(!txt) return;
    if(!db.chat[sessao]) db.chat[sessao] = [];
    db.chat[sessao].push({ texto: txt, autor: 'cliente' });
    salvar();
    document.getElementById("chat_input").value = "";
    rendChat();
}

function rendChat() {
    const m = db.chat[sessao] || [];
    document.getElementById("chat_mensagens").innerHTML = m.map(msg => `
        <div style="padding:10px; border-radius:10px; max-width:80%; background:${msg.autor==='cliente'?'#dcf8c6':'#fff'}; align-self:${msg.autor==='cliente'?'flex-end':'flex-start'}">${msg.texto}</div>
    `).join('');
}

function voltarDoChat() {
    if(sessao.includes("visitante")) irPara('tela_login');
    else irPara('tela_home');
}

window.onload = () => { document.getElementById("txt_loja_nome").innerText = db.config.nome; };
</script>
</body>
</html>
