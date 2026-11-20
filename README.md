<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#f7f8f4">
  <title>KINORA — Carrinho Inteligente</title>

  <style>
    :root {
      --verde: #176b4d;
      --verde-escuro: #104b38;
      --verde-claro: #e8f4ed;
      --fundo: #f7f8f4;
      --texto: #1d2923;
      --secundario: #647168;
      --borda: #dfe6df;
      --branco: #fff;
      --amarelo: #fff4d6;
      --vermelho: #a33333;
      --sombra: 0 12px 32px rgba(24, 54, 39, .08);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: var(--fundo);
      color: var(--texto);
      line-height: 1.5;
    }

    button, input, select { font: inherit; }

    button {
      cursor: pointer;
      border: 0;
    }

    button:focus-visible, a:focus-visible, select:focus-visible {
      outline: 3px solid #e0a82e;
      outline-offset: 3px;
    }

    .topbar {
      background: var(--branco);
      border-bottom: 1px solid var(--borda);
      position: sticky;
      top: 0;
      z-index: 5;
    }

    .topbar-inner {
      max-width: 1120px;
      min-height: 74px;
      margin: auto;
      padding: 12px 22px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 11px;
      font-weight: 850;
      letter-spacing: .08em;
      color: var(--verde-escuro);
    }

    .brand-mark {
      width: 40px;
      height: 40px;
      border-radius: 14px;
      background: var(--verde);
      color: white;
      display: grid;
      place-items: center;
      font-size: 21px;
    }

    .brand small {
      display: block;
      font-size: 11px;
      letter-spacing: .02em;
      font-weight: 500;
      color: var(--secundario);
    }

    .mode-switch {
      display: flex;
      gap: 6px;
      background: #f0f3ef;
      border-radius: 14px;
      padding: 5px;
    }

    .mode-switch button {
      padding: 10px 14px;
      min-height: 44px;
      border-radius: 10px;
      color: var(--secundario);
      background: transparent;
      font-weight: 700;
    }

    .mode-switch button.active {
      background: var(--verde);
      color: white;
    }

    main {
      max-width: 1120px;
      margin: auto;
      padding: 30px 22px 56px;
    }

    .view { display: none; }
    .view.active { display: block; }

    .hero {
      background: linear-gradient(135deg, #e8f4ed, #f3f6e8);
      border: 1px solid #d8e9dc;
      border-radius: 24px;
      padding: clamp(24px, 5vw, 54px);
      display: grid;
      grid-template-columns: 1.2fr .8fr;
      gap: 28px;
      align-items: center;
      box-shadow: var(--sombra);
    }

    .eyebrow {
      text-transform: uppercase;
      color: var(--verde);
      font-size: 12px;
      font-weight: 800;
      letter-spacing: .12em;
    }

    h1, h2, h3, p { margin-top: 0; }
    h1 { font-size: clamp(34px, 5vw, 54px); line-height: 1.05; margin-bottom: 16px; }
    h2 { font-size: clamp(24px, 3vw, 32px); margin-bottom: 8px; }
    h3 { font-size: 18px; margin-bottom: 8px; }

    .muted { color: var(--secundario); }

    .hero-art {
      min-height: 250px;
      border-radius: 22px;
      background: #d7eadc;
      display: grid;
      place-items: center;
      text-align: center;
      padding: 20px;
      color: var(--verde-escuro);
    }

    .cart-illustration { font-size: 105px; line-height: 1; }
    .hero-art small { display: block; margin-top: 12px; }

    .actions {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 24px;
    }

    .btn {
      min-height: 48px;
      padding: 12px 18px;
      border-radius: 12px;
      font-weight: 800;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      text-decoration: none;
    }

    .btn-primary { background: var(--verde); color: white; }
    .btn-primary:hover { background: var(--verde-escuro); }
    .btn-secondary { background: white; color: var(--verde-escuro); border: 1px solid var(--borda); }
    .btn-danger { background: #fff0ef; color: var(--vermelho); }
    .btn:disabled { opacity: .5; cursor: not-allowed; }

    .notice {
      margin: 18px 0 0;
      border-left: 4px solid #d59d21;
      background: var(--amarelo);
      border-radius: 10px;
      padding: 13px 15px;
      color: #5b4818;
      font-size: 14px;
    }

    .section-head {
      margin: 30px 0 16px;
      display: flex;
      justify-content: space-between;
      align-items: end;
      gap: 16px;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
    }

    .card {
      background: white;
      border: 1px solid var(--borda);
      border-radius: 18px;
      padding: 20px;
      box-shadow: 0 6px 20px rgba(24, 54, 39, .04);
    }

    .card-icon {
      width: 44px;
      height: 44px;
      border-radius: 14px;
      background: var(--verde-claro);
      display: grid;
      place-items: center;
      font-size: 23px;
      margin-bottom: 14px;
    }

    .screen-title {
      display: flex;
      align-items: end;
      justify-content: space-between;
      gap: 16px;
      margin-bottom: 22px;
    }

    .layout {
      display: grid;
      grid-template-columns: 1.5fr .8fr;
      gap: 18px;
      align-items: start;
    }

    .product-row {
      display: grid;
      grid-template-columns: 1fr auto auto;
      gap: 14px;
      align-items: center;
      padding: 15px 0;
      border-bottom: 1px solid var(--borda);
    }

    .product-row:last-child { border-bottom: 0; }
    .product-name { font-weight: 800; }
    .product-meta { font-size: 13px; color: var(--secundario); }

    .qty-controls {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .qty-controls button {
      width: 38px;
      height: 38px;
      border-radius: 10px;
      background: #eef4ef;
      color: var(--verde-escuro);
      font-size: 19px;
      font-weight: 800;
    }

    .summary-line {
      display: flex;
      justify-content: space-between;
      gap: 16px;
      margin: 10px 0;
    }

    .summary-total {
      border-top: 1px solid var(--borda);
      padding-top: 15px;
      margin-top: 15px;
      font-size: 21px;
      font-weight: 900;
    }

    .status {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      border-radius: 999px;
      padding: 7px 11px;
      background: var(--verde-claro);
      color: var(--verde-escuro);
      font-size: 13px;
      font-weight: 800;
    }

    .status.warning { background: var(--amarelo); color: #654d12; }
    .status.offline { background: #fff0ef; color: var(--vermelho); }

    .scan-panel {
      display: grid;
      gap: 12px;
    }

    select {
      width: 100%;
      min-height: 48px;
      padding: 10px 12px;
      border: 1px solid var(--borda);
      border-radius: 11px;
      background: white;
      color: var(--texto);
    }

    .feedback {
      padding: 14px;
      margin-top: 12px;
      border-radius: 12px;
      background: var(--verde-claro);
      color: var(--verde-escuro);
      font-weight: 650;
    }

    .feedback.error {
      background: #fff0ef;
      color: var(--vermelho);
    }

    .empty {
      padding: 28px 12px;
      text-align: center;
      color: var(--secundario);
    }

    .dashboard-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 14px;
      margin-bottom: 16px;
    }

    .metric-label { color: var(--secundario); font-size: 13px; }
    .metric-value { font-size: 28px; font-weight: 900; margin: 7px 0 2px; color: var(--verde-escuro); }

    .device-row, .alert-row {
      display: flex;
      justify-content: space-between;
      gap: 14px;
      align-items: center;
      padding: 12px 0;
      border-bottom: 1px solid var(--borda);
    }

    .device-row:last-child, .alert-row:last-child { border-bottom: 0; }
    .alert-row { align-items: start; }

    .bar-chart {
      height: 170px;
      display: flex;
      align-items: end;
      gap: 12px;
      padding: 12px 4px 0;
      border-bottom: 1px solid var(--borda);
    }

    .bar {
      flex: 1;
      min-width: 18px;
      background: linear-gradient(180deg, #61a981, var(--verde));
      border-radius: 8px 8px 0 0;
      position: relative;
    }

    .bar span {
      position: absolute;
      bottom: -25px;
      left: 50%;
      transform: translateX(-50%);
      color: var(--secundario);
      font-size: 11px;
    }

    footer {
      border-top: 1px solid var(--borda);
      padding: 20px;
      text-align: center;
      color: var(--secundario);
      font-size: 13px;
    }

    .hidden { display: none !important; }

    @media (max-width: 760px) {
      .topbar-inner { align-items: flex-start; flex-direction: column; }
      .mode-switch { width: 100%; }
      .mode-switch button { flex: 1; padding: 9px 8px; }
      .hero, .layout { grid-template-columns: 1fr; }
      .grid { grid-template-columns: 1fr; }
      .dashboard-grid { grid-template-columns: repeat(2, 1fr); }
      .product-row { grid-template-columns: 1fr auto; }
      .product-row .qty-controls { grid-column: 1 / -1; }
      main { padding: 20px 14px 40px; }
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { scroll-behavior: auto !important; transition: none !important; }
    }
  </style>
</head>

<body>
  <header class="topbar">
    <div class="topbar-inner">
      <div class="brand" aria-label="KINORA">
        <span class="brand-mark" aria-hidden="true">K</span>
        <span>KINORA<small>Mais rapidez, menos filas.</small></span>
      </div>

      <nav class="mode-switch" aria-label="Selecionar área do protótipo">
        <button id="customerMode" class="active" type="button">Cliente / carrinho</button>
        <button id="managerMode" type="button">Gestão do supermercado</button>
      </nav>
    </div>
  </header>

  <main>
    <section id="customerView" class="view active" aria-live="polite">
      <section id="homeScreen">
        <div class="hero">
          <div>
            <div class="eyebrow">Carrinho inteligente de supermercado</div>
            <h1>Compre no seu ritmo. Sem fila no caixa.</h1>
            <p class="muted">
              Escaneie os produtos, acompanhe o total da compra e revise seus itens
              diretamente pelo carrinho KINORA.
            </p>
            <div class="status" aria-label="Status demonstrativo: carrinho conectado">
              ● Conexão demonstrativa ativa
            </div>
            <div class="actions">
              <button class="btn btn-primary" type="button" onclick="startShopping()">Começar compra</button>
              <button class="btn btn-secondary" type="button" onclick="showScreen('cartScreen')">Minha lista</button>
            </div>
          </div>
          <div class="hero-art" aria-label="Ilustração conceitual de um carrinho de supermercado">
            <div>
              <div class="cart-illustration" aria-hidden="true">🛒</div>
              <strong>Tela touchscreen + leitor</strong>
              <small>Representação conceitual. Não é hardware funcional.</small>
            </div>
          </div>
        </div>

        <div class="notice">
          Protótipo acadêmico: leitura de produtos, conexão, sensor de peso e checkout
          são simulações. Nenhum pagamento real é realizado.
        </div>

        <div class="section-head">
          <div>
            <div class="eyebrow">Experiência de compra</div>
            <h2>Como funciona</h2>
          </div>
        </div>

        <div class="grid">
          <article class="card">
            <div class="card-icon" aria-hidden="true">▦</div>
            <h3>1. Escaneie</h3>
            <p class="muted">Simule a leitura do código de barras de um produto.</p>
          </article>
          <article class="card">
            <div class="card-icon" aria-hidden="true">⚖</div>
            <h3>2. Confira</h3>
            <p class="muted">Veja a validação demonstrativa do item e do peso.</p>
          </article>
          <article class="card">
            <div class="card-icon" aria-hidden="true">✓</div>
            <h3>3. Revise e finalize</h3>
            <p class="muted">Confira a lista e conclua uma compra de demonstração.</p>
          </article>
        </div>
      </section>

      <section id="cartScreen" class="hidden">
        <div class="screen-title">
          <div>
            <div class="eyebrow">Compra em andamento</div>
            <h2>Seu carrinho</h2>
            <p class="muted">Os itens e preços abaixo são demonstrativos.</p>
          </div>
          <button class="btn btn-secondary" type="button" onclick="showScreen('homeScreen')">Voltar ao início</button>
        </div>

        <div class="layout">
          <div class="card">
            <h3>Itens selecionados</h3>
            <div id="cartItems"></div>
            <div id="emptyCart" class="empty">Sua lista está vazia. Simule a leitura de um produto para começar.</div>
          </div>

          <aside class="card">
            <h3>Adicionar produto</h3>
            <p class="muted">Selecione um item para simular a leitura.</p>
            <div class="scan-panel">
              <label for="productSelect"><strong>Produto demonstrativo</strong></label>
              <select id="productSelect">
                <option value="apple">Maçãs — pacote</option>
                <option value="bread">Pão de forma</option>
                <option value="milk">Leite integral</option>
              </select>

              <label for="weightMode"><strong>Resultado da validação simulada</strong></label>
              <select id="weightMode">
                <option value="ok">Item validado</option>
                <option value="error">Simular divergência de peso</option>
              </select>

              <button class="btn btn-primary" type="button" onclick="simulateScan()">Escanear produto</button>
            </div>

            <div id="scanFeedback" role="status" aria-live="polite"></div>
            <div class="notice">O leitor e o sensor de peso não estão conectados a dispositivos reais.</div>
          </aside>
        </div>

        <div class="layout" style="margin-top:18px">
          <div class="card">
            <h3>Validação por peso</h3>
            <p class="muted">A simulação informa se os itens foram validados ou se precisam de conferência.</p>
            <div id="validationStatus" class="status">Nenhum item validado</div>
          </div>

          <aside class="card">
            <h3>Resumo</h3>
            <div class="summary-line"><span>Itens</span><strong id="itemCount">0</strong></div>
            <div class="summary-line"><span>Subtotal demonstrativo</span><strong id="subtotal">R$ 0,00</strong></div>
            <div class="summary-line summary-total"><span>Total</span><strong id="total">R$ 0,00</strong></div>
            <div class="actions">
              <button class="btn btn-secondary" type="button" onclick="showScreen('cartScreen')">Revisar carrinho</button>
              <button id="checkoutButton" class="btn btn-primary" type="button" onclick="showScreen('checkoutScreen')">Ver resumo</button>
            </div>
          </aside>
        </div>
      </section>

      <section id="checkoutScreen" class="hidden">
        <div class="screen-title">
          <div>
            <div class="eyebrow">Resumo e confirmação</div>
            <h2>Confira antes de finalizar</h2>
            <p class="muted">Esta etapa é demonstrativa e não realiza cobrança.</p>
          </div>
          <button class="btn btn-secondary" type="button" onclick="showScreen('cartScreen')">Voltar ao carrinho</button>
        </div>

        <div class="layout">
          <div class="card">
            <h3>Lista da compra</h3>
            <div id="checkoutItems"></div>
          </div>
          <aside class="card">
            <h3>Resumo do pedido</h3>
            <div class="summary-line"><span>Itens</span><strong id="checkoutCount">0</strong></div>
            <div class="summary-line"><span>Validação</span><strong id="checkoutValidation">Pendente</strong></div>
            <div class="summary-line summary-total"><span>Total demonstrativo</span><strong id="checkoutTotal">R$ 0,00</strong></div>
            <div class="notice">Ao concluir, você verá uma confirmação de demonstração. Nenhum pagamento será processado.</div>
            <div class="actions">
              <button class="btn btn-primary" type="button" onclick="finishPurchase()">Concluir demonstração</button>
            </div>
          </aside>
        </div>
      </section>

      <section id="successScreen" class="hidden">
        <div class="card" style="max-width:700px;margin:30px auto;text-align:center;padding:40px 24px">
          <div class="card-icon" style="margin:0 auto 16px;font-size:30px" aria-hidden="true">✓</div>
          <div class="eyebrow">Simulação concluída</div>
          <h2>Compra demonstrativa finalizada!</h2>
          <p class="muted">Este protótipo não cobrou nem processou pagamento.</p>
          <p><strong>Pedido:</strong> <span id="orderCode">KIN-DEMO</span></p>
          <p><strong>Total ilustrativo:</strong> <span id="successTotal">R$ 0,00</span></p>
          <button class="btn btn-primary" type="button" onclick="resetPurchase()">Voltar ao início</button>
        </div>
      </section>
    </section>

    <section id="managerView" class="view">
      <div class="screen-title">
        <div>
          <div class="eyebrow">Painel de gestão</div>
          <h2>Supermercado Vila Verde</h2>
          <p class="muted">Indicadores e alertas ilustrativos do protótipo.</p>
        </div>
        <span class="status warning">Dados fictícios de demonstração</span>
      </div>

      <div class="notice">
        Os valores deste painel são inventados para demonstrar a interface. Não representam
        vendas reais da KINORA nem de um supermercado.
      </div>

      <div class="dashboard-grid" style="margin-top:18px">
        <article class="card">
          <div class="metric-label">Carrinhos ativos</div>
          <div class="metric-value">12</div>
          <div class="metric-label">Dado demonstrativo</div>
        </article>
        <article class="card">
          <div class="metric-label">Vendas do dia</div>
          <div class="metric-value">R$ 18.430</div>
          <div class="metric-label">Valor fictício</div>
        </article>
        <article class="card">
          <div class="metric-label">Compras concluídas</div>
          <div class="metric-value">146</div>
          <div class="metric-label">Dado demonstrativo</div>
        </article>
        <article class="card">
          <div class="metric-label">Tempo médio</div>
          <div class="metric-value">6 min</div>
          <div class="metric-label">Estimativa ilustrativa</div>
        </article>
      </div>

      <div class="layout">
        <article class="card">
          <h3>Tendência de vendas demonstrativa</h3>
          <p class="muted">Representação visual fictícia, sem dados conectados.</p>
          <div class="bar-chart" role="img" aria-label="Gráfico ilustrativo de vendas ao longo do dia">
            <div class="bar" style="height:35%"><span>08h</span></div>
            <div class="bar" style="height:55%"><span>10h</span></div>
            <div class="bar" style="height:45%"><span>12h</span></div>
            <div class="bar" style="height:70%"><span>14h</span></div>
            <div class="bar" style="height:62%"><span>16h</span></div>
            <div class="bar" style="height:88%"><span>18h</span></div>
            <div class="bar" style="height:76%"><span>20h</span></div>
          </div>
        </article>

        <article class="card">
          <h3>Status dos dispositivos</h3>
          <div class="device-row"><span>Carrinhos</span><span class="status">11 de 12 online</span></div>
          <div class="device-row"><span>Leitores</span><span class="status">Simulação OK</span></div>
          <div class="device-row"><span>Balança C-07</span><span class="status warning">Calibração pendente</span></div>
          <div class="device-row"><span>Rede Wi-Fi</span><span class="status">Estável (demo)</span></div>
        </article>
      </div>

      <article class="card" style="margin-top:18px">
        <h3>Alertas operacionais demonstrativos</h3>
        <div class="alert-row">
          <div><strong>Divergência de peso no carrinho 04</strong><br><span class="muted">Recomenda-se conferir o item antes de continuar.</span></div>
          <span class="status warning">Atenção</span>
        </div>
        <div class="alert-row">
          <div><strong>Balança C-07 com calibração pendente</strong><br><span class="muted">Alerta ilustrativo para manutenção.</span></div>
          <span class="status warning">Verificar</span>
        </div>
        <div class="alert-row">
          <div><strong>Rede estável nos corredores</strong><br><span class="muted">Status fictício para demonstração.</span></div>
          <span class="status">Informativo</span>
        </div>
      </article>
    </section>

    <section class="card" style="margin-top:24px">
      <h3>Sobre este protótipo</h3>
      <p class="muted">
        KINORA — atividade acadêmica de Prototipagem das Telas, PUC-Campinas,
        Ciência de Dados e Inteligência Artificial, Gerenciamento de Projetos, 2026.
        Protótipo conceitual; não conectado a hardware, balança, sistema de loja ou serviço de pagamento.
      </p>
    </section>
  </main>

  <footer>
    KINORA • Protótipo acadêmico • “Mais rapidez, menos filas.”
  </footer>

  <script>
    const products = {
      apple: { id: "apple", name: "Maçãs — pacote", price: 2.99 },
      bread: { id: "bread", name: "Pão de forma", price: 5.49 },
      milk: { id: "milk", name: "Leite integral", price: 4.99 }
    };

    let cart = [];
    let allValidated = true;
    let nextScanHasMismatch = false;

    const money = value =>
      new Intl.NumberFormat("pt-BR", { style: "currency", currency: "BRL" }).format(value);

    function setMode(mode) {
      const customer = mode === "customer";
      document.getElementById("customerView").classList.toggle("active", customer);
      document.getElementById("managerView").classList.toggle("active", !customer);
      document.getElementById("customerMode").classList.toggle("active", customer);
      document.getElementById("managerMode").classList.toggle("active", !customer);
    }

    document.getElementById("customerMode").addEventListener("click", () => setMode("customer"));
    document.getElementById("managerMode").addEventListener("click", () => setMode("manager"));

    function showScreen(screenId) {
      ["homeScreen", "cartScreen", "checkoutScreen", "successScreen"].forEach(id => {
        document.getElementById(id).classList.toggle("hidden", id !== screenId);
      });

      if (screenId === "cartScreen") renderCart();
      if (screenId === "checkoutScreen") renderCheckout();

      window.scrollTo({ top: 0, behavior: "smooth" });
    }

    function startShopping() {
      setMode("customer");
      showScreen("cartScreen");
    }

    function simulateScan() {
      const id = document.getElementById("productSelect").value;
      const validationMode = document.getElementById("weightMode").value;
      const product = products[id];
      const feedback = document.getElementById("scanFeedback");

      if (validationMode === "error") {
        allValidated = false;
        feedback.innerHTML =
          '<div class="feedback error"><strong>Divergência simulada.</strong> Confira o produto e tente validar novamente.</div>' +
          '<div class="actions"><button class="btn btn-secondary" type="button" onclick="resolveMismatch()">Corrigir e validar item</button></div>';
        updateValidation();
        return;
      }

      const existing = cart.find(item => item.id === product.id);
      if (existing) {
        existing.quantity += 1;
      } else {
        cart.push({ ...product, quantity: 1 });
      }

      allValidated = true;
      feedback.innerHTML =
        '<div class="feedback"><strong>Produto validado.</strong> Item adicionado à lista. Leitura e peso são simulações.</div>';
      renderCart();
      updateValidation();
    }

    function resolveMismatch() {
      allValidated = true;
      document.getElementById("weightMode").value = "ok";
      const id = document.getElementById("productSelect").value;
      const product = products[id];
      const existing = cart.find(item => item.id === product.id);

      if (existing) existing.quantity += 1;
      else cart.push({ ...product, quantity: 1 });

      document.getElementById("scanFeedback").innerHTML =
        '<div class="feedback"><strong>Item corrigido e validado.</strong> Resultado apenas demonstrativo.</div>';

      renderCart();
      updateValidation();
    }

    function changeQuantity(id, amount) {
      const item = cart.find(product => product.id === id);
      if (!item) return;

      item.quantity += amount;
      if (item.quantity <= 0) cart = cart.filter(product => product.id !== id);

      renderCart();
      renderCheckout();
    }

    function removeItem(id) {
      cart = cart.filter(item => item.id !== id);
      renderCart();
      renderCheckout();
    }

    function totalItems() {
      return cart.reduce((sum, item) => sum + item.quantity, 0);
    }

    function totalPrice() {
      return cart.reduce((sum, item) => sum + item.quantity * item.price, 0);
    }

    function renderCart() {
      const container = document.getElementById("cartItems");
      const empty = document.getElementById("emptyCart");

      if (!cart.length) {
        container.innerHTML = "";
        empty.classList.remove("hidden");
      } else {
        empty.classList.add("hidden");
        container.innerHTML = cart.map(item => `
          <div class="product-row">
            <div>
              <div class="product-name">${item.name}</div>
              <div class="product-meta">${money(item.price)} por unidade • preço demonstrativo</div>
            </div>
            <strong>${money(item.price * item.quantity)}</strong>
            <div class="qty-controls" aria-label="Alterar quantidade de ${item.name}">
              <button type="button" aria-label="Diminuir quantidade" onclick="changeQuantity('${item.id}', -1)">−</button>
              <strong>${item.quantity}</strong>
              <button type="button" aria-label="Aumentar quantidade" onclick="changeQuantity('${item.id}', 1)">+</button>
              <button class="btn-danger" type="button" aria-label="Remover ${item.name}" onclick="removeItem('${item.id}')">×</button>
            </div>
          </div>
        `).join("");
      }

      document.getElementById("itemCount").textContent = totalItems();
      document.getElementById("subtotal").textContent = money(totalPrice());
      document.getElementById("total").textContent = money(totalPrice());

      const checkoutButton = document.getElementById("checkoutButton");
      checkoutButton.disabled = cart.length === 0;

      updateValidation();
    }

    function updateValidation() {
      const status = document.getElementById("validationStatus");
      if (!cart.length) {
        status.textContent = "Nenhum item validado";
        status.className = "status";
      } else if (!allValidated) {
        status.textContent = "Há item pendente de conferência";
        status.className = "status offline";
      } else {
        status.textContent = "Itens validados (simulação)";
        status.className = "status";
      }
    }

    function renderCheckout() {
      const list = document.getElementById("checkoutItems");
      list.innerHTML = cart.length ? cart.map(item => `
        <div class="product-row">
          <div>
            <div class="product-name">${item.name}</div>
            <div class="product-meta">${item.quantity} unidade(s) × ${money(item.price)}</div>
          </div>
          <strong>${money(item.quantity * item.price)}</strong>
        </div>
      `).join("") : '<div class="empty">Nenhum item no carrinho.</div>';

      document.getElementById("checkoutCount").textContent = totalItems();
      document.getElementById("checkoutTotal").textContent = money(totalPrice());
      document.getElementById("checkoutValidation").textContent =
        cart.length && allValidated ? "Validado (simulação)" : "Pendente";
    }

    function finishPurchase() {
      if (!cart.length) {
        alert("Adicione pelo menos um produto demonstrativo antes de concluir.");
        showScreen("cartScreen");
        return;
      }

      if (!allValidated) {
        alert("Resolva a divergência simulada antes de concluir.");
        showScreen("cartScreen");
        return;
      }

      document.getElementById("successTotal").textContent = money(totalPrice());
      document.getElementById("orderCode").textContent =
        "KIN-DEMO-" + Math.floor(1000 + Math.random() * 9000);
      showScreen("successScreen");
    }

    function resetPurchase() {
      cart = [];
      allValidated = true;
      document.getElementById("scanFeedback").innerHTML = "";
      setMode("customer");
      showScreen("homeScreen");
    }

    renderCart();
  </script>
</body>
</html>
