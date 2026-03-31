<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#e53935">
    <title id="siteTitle">Medela Supermercado - App</title>
    <style>
        :root { --primary: #e53935; --admin-bg: #2c3e50; --bg: #f4f4f4; --whatsapp: #25d366; --caixa: #f39c12; }
        * { box-sizing: border-box; font-family: 'Segoe UI', Arial, sans-serif; -webkit-tap-highlight-color: transparent; }
        body { margin: 0; background: var(--bg); color: #333; overflow-x: hidden; }
        
        /* Telas */
        .tela { display: none; min-height: 100vh; width: 100%; }
        .ativa { display: block !important; }
        .container { max-width: 500px; margin: auto; padding: 15px; }
        .card { background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); margin-bottom: 15px; }
        
        /* UI Elements */
        input, select, textarea { width: 100%; padding: 14px; margin: 10px 0; border: 1px solid #ccc; border-radius: 10px; font-size: 16px; outline: none; }
        button { width: 100%; padding: 14px; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; font-size: 16px; color: white; margin-top: 5px; }
        button:active { transform: scale(0.96); opacity: 0.8; }
        
        .btn-main { background: var(--primary); }
        .btn-sec { background: var(--admin-bg); }
        .btn-caixa { background: var(--caixa); }
        .btn-whats { background: var(--whatsapp); display: flex; align-items: center; justify-content: center; gap: 10px; }
        
        .header { background: var(--primary); color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; height: 60px; }
        
        /* Tabs Admin */
        .tab-menu { display: flex; background: #fff; margin-bottom: 15px; border-radius: 10px; overflow: hidden; border: 1px solid #ddd; }
        .tab-btn { flex: 1; padding: 12px; background: #f9f9f9; color: #333; border-radius: 0; margin: 0; font-size: 13px; }
        .tab-btn.active { background: var(--admin-bg); color: white; }

        /* Grid */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .prod-card { background: white; padding: 15px; border-radius: 12px; text-align: center; border: 1px solid #eee; }
    </style>
</head>
<body>

<div id="tela_login" class="tela ativa">
    <div class="container" style="padding-top: 50px; text-align: center;">
        <h1 id="txt_loja_nome">Medela Supermercado</h1>
        <div class="card">
            <input type="text" id="login_cpf" placeholder="CPF, 'admin' ou 'caixa'">
            <input type="password" id="login_senha" placeholder="Senha">
            <button type="button" class="btn-main" onclick="funcaoLogin(event)">ENTRAR NO APP</button>
            <button type="button" class="btn-whats" onclick="irPara('tela_suporte_aberto')">💬 SUPORTE CHAT</button>
            <p onclick="irPara('tela_cadastro')" style="margin-top:20px; color:var(--primary); cursor:pointer; font-weight:bold;">Criar Conta de Cliente</p>
        </div>
    </div>
</div>

<div id="tela_cadastro" class="tela">
    <div class="container">
        <h2>Nova Conta</h2>
        <div class="card">
            <input id="cad_nome" placeholder="Nome Completo">
            <input id="cad_cpf" placeholder="CPF">
            <input id="cad_senha" type="password" placeholder="Crie uma Senha">
            <button type="button" class="btn-main" onclick="funcaoCadastro(event)">CADASTRAR</button>
            <button type="button" class="btn-sec" onclick="irPara('tela_login')">VOLTAR</button>
        </div>
    </div>
</div>

<div id="tela_home" class="tela">
    <div class="header">
        <span id="txt_boas_vindas">Olá!</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(0,0,0,0.2); padding:8px 15px">Sair</button>
    </div>
    <div class="container">
        <div class="grid" id="lista_produtos"></div>
    </div>
</div>

<div id="tela_caixa" class="tela">
    <div class="header" style="background:var(--caixa)">
        <span>SISTEMA DE CAIXA</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(0,0,0,0.2)">Sair</button>
    </div>
    <div class="container">
        <div class="card">
            <h3>Venda Presencial</h3>
            <select id="caixa_select_prod"></select>
            <button type="button" class="btn-caixa" onclick="caixaAddProd()">ADICIONAR ITEM</button>
        </div>
        <div id="caixa_lista" class="card"></div>
        <div class="card">
            <h2 id="caixa_total">Total: R$ 0,00</h2>
            <button type="button" class="btn-main" onclick="caixaFinalizar()">CONCLUIR VENDA NO CAIXA</button>
        </div>
    </div>
</div>

<div id="tela_admin" class="tela">
    <div class="header" style="background:var(--admin-bg)">
        <span>GERÊNCIA</span>
        <button type="button" onclick="logout()" style="width:auto; margin:0; background:rgba(255,255,255,0.1)">Sair</button>
    </div>
    <div class="container">
        <div class="tab-menu">
            <button class="tab-btn active" onclick="mudarTab('tab_financeiro', this)">Financeiro</button>
            <button class="tab-btn" onclick="mudarTab('tab_pedidos', this)">Vendas</button>
            <button class="tab-btn" onclick="mudarTab('tab_suporte', this)">Suporte</button>
        </div>

        <div id="tab_financeiro" class="tab-content">
            <div style="display:flex; gap:10px; margin-bottom:15px;">
                <div style="flex:1; background:#2ecc71; color:white; padding:15px; border-radius:10px; text-align:center;">
                    <small>BRUTO (Venda)</small><br><strong id="adm_bruto">R$ 0,00</strong>
                </div>
                <div style="flex:1; background:#3498db; color:white; padding:15px; border-radius:10px; text-align:center;">
                    <small>LÍQUIDO (Lucro)</small><br><strong id="adm_liquido">R$ 0,00</strong>
                </div>
            </div>
            <div class="card">
                <h3>Cadastrar Produto</h3>
                <input id="adm_p_nome" placeholder="Nome do Produto">
                <input id="adm_p_custo" type="number" step="0.01" placeholder="Custo (Valor Líquido)">
                <input id="adm_p_venda" type="number" step="0.01" placeholder="Venda (Valor Bruto)">
                <button type="button" class="btn-main" onclick="admAddProduto(event)">SALVAR PRODUTO</button>
            </div>
        </div>

        <div id="tab_pedidos" class="tab-content" style="display:none">
            <div id="adm_lista_pedidos"></div>
        </div>

        <div id="tab_suporte" class="tab-content" style="display:none">
            <div id="adm_lista_chats"></div>
        </div>
    </div>
</div>

<script>
// DB COMPLETA
let db = {
    users: JSON.parse(localStorage.getItem("db_users")) || {},
    prods: JSON.parse(localStorage.getItem("db_prods")) || [],
    pedidos: JSON.parse(localStorage.getItem("db_pedidos")) || [],
    config: JSON.parse(localStorage.getItem("db_config")) || { nome: "Medela Supermercado", whats: "" }
};

function salvar() { localStorage.setItem("db_users", JSON.stringify(db.users)); localStorage.setItem("db_prods", JSON.stringify(db.prods)); localStorage.setItem("db_pedidos", JSON.stringify(db.pedidos)); localStorage.setItem("db_config", JSON.stringify(db.config)); }

let sessao = ""; let carrinhoCaixa = [];

function irPara(id) {
    document.querySelectorAll('.tela').forEach(t => t.classList.remove('ativa'));
    document.getElementById(id).classList.add('ativa');
    if(id === 'tela_home') rendHome();
    if(id === 'tela_admin') rendAdmin();
    if(id === 'tela_caixa') rendCaixa();
}

function mudarTab(id, btn) {
    document.querySelectorAll('.tab-content').forEach(c => c.style.display = 'none');
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    document.getElementById(id).style.display = 'block';
    btn.classList.add('active');
}

function funcaoLogin(e) {
    e.preventDefault();
    const c = document.getElementById("login_cpf").value;
    const s = document.getElementById("login_senha").value;
    if(c === 'admin' && s === 'admin123') { sessao = "admin"; irPara('tela_admin'); }
    else if(c === 'caixa' && s === 'caixa123') { sessao = "caixa"; irPara('tela_caixa'); }
    else if(db.users[c] && db.users[c].senha === s) { sessao = c; irPara('tela_home'); }
    else alert("Usuário ou senha inválidos!");
}

function funcaoCadastro(e) {
    e.preventDefault();
    const n = document.getElementById("cad_nome").value;
    const c = document.getElementById("cad_cpf").value;
    const s = document.getElementById("cad_senha").value;
    if(!n || !c || !s) return alert("Preencha todos os campos!");
    db.users[c] = { nome: n, senha: s };
    salvar();
    alert("Conta criada com sucesso!");
    irPara('tela_login');
}

function rendHome() {
    document.getElementById("txt_boas_vindas").innerText = "Olá, " + (db.users[sessao]?.nome || "Cliente");
    const lista = document.getElementById("lista_produtos");
    lista.innerHTML = db.prods.map(p => `<div class="prod-card"><strong>${p.nome}</strong><br><span style="color:green; font-weight:bold;">R$ ${p.venda.toFixed(2)}</span><button class="btn-main" style="font-size:12px; padding:8px;">🛒 COMPRAR</button></div>`).join('') || "Carregando produtos...";
}

function rendCaixa() {
    const sel = document.getElementById("caixa_select_prod");
    sel.innerHTML = db.prods.map((p, i) => `<option value="${i}">${p.nome} - R$ ${p.venda.toFixed(2)}</option>`).join('');
    caixaAtualizarLista();
}

function caixaAddProd() {
    const idx = document.getElementById("caixa_select_prod").value;
    if(db.prods[idx]) { carrinhoCaixa.push(db.prods[idx]); caixaAtualizarLista(); }
}

function caixaAtualizarLista() {
    let total = carrinhoCaixa.reduce((a, b) => a + b.venda, 0);
    document.getElementById("caixa_lista").innerHTML = carrinhoCaixa.map(i => `<div style="border-bottom:1px solid #eee; padding:5px;">${i.nome} - R$ ${i.venda.toFixed(2)}</div>`).join('') || "Nenhum item adicionado";
    document.getElementById("caixa_total").innerText = "Total: R$ " + total.toFixed(2);
}

function caixaFinalizar() {
    if(!carrinhoCaixa.length) return alert("Adicione produtos primeiro!");
    let bruto = carrinhoCaixa.reduce((a, b) => a + b.venda, 0);
    let custo = carrinhoCaixa.reduce((a, b) => a + b.custo, 0);
    db.pedidos.unshift({ id: "CX-"+Date.now(), bruto: bruto, liquido: bruto - custo, itens: "Venda Direta no Caixa", status: "Concluído" });
    salvar();
    carrinhoCaixa = [];
    caixaAtualizarLista();
    alert("Venda registrada no sistema!");
}

function rendAdmin() {
    document.getElementById("adm_bruto").innerText = "R$ " + db.pedidos.reduce((a, b) => a + b.bruto, 0).toFixed(2);
    document.getElementById("adm_liquido").innerText = "R$ " + db.pedidos.reduce((a, b) => a + b.liquido, 0).toFixed(2);
    document.getElementById("adm_lista_pedidos").innerHTML = db.pedidos.map(p => `<div class="card"><b>ID: ${p.id}</b><br>Valor Bruto: R$ ${p.bruto.toFixed(2)} | Lucro: R$ ${p.liquido.toFixed(2)}</div>`).join('') || "Nenhuma venda registrada.";
}

function admAddProduto(e) {
    e.preventDefault();
    const n = document.getElementById("adm_p_nome").value;
    const c = parseFloat(document.getElementById("adm_p_custo").value);
    const v = parseFloat(document.getElementById("adm_p_venda").value);
    if(n && !isNaN(c) && !isNaN(v)) { db.prods.push({ nome: n, custo: c, venda: v }); salvar(); alert("Produto Cadastrado!"); rendAdmin(); }
    else alert("Verifique os valores informados!");
}

function logout() { sessao = ""; irPara('tela_login'); }
window.onload = () => { document.getElementById("txt_loja_nome").innerText = db.config.nome; };
</script>
</body>
</html>
