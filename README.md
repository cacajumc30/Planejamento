<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard de Planejamento Pessoal</title>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* (mesmos estilos das versões anteriores) */
        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
        }
        body {
            background: #f0f2f5;
            padding: 20px;
        }
        .container {
            max-width: 1400px;
            margin: 0 auto;
        }
        h1 {
            color: #1e1e2f;
            margin-bottom: 10px;
            font-weight: 600;
        }
        .backup-bar {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            padding: 10px;
            background: #e9ecef;
            border-radius: 8px;
            align-items: center;
            flex-wrap: wrap;
        }
        .backup-bar button {
            background: #28a745;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
            font-size: 14px;
            margin: 0;
        }
        .backup-bar button:hover {
            background: #218838;
        }
        .backup-bar .import-label {
            background: #007bff;
            color: white;
            padding: 8px 16px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
            font-size: 14px;
        }
        .backup-bar .import-label:hover {
            background: #0069d9;
        }
        #file-import {
            display: none;
        }
        .tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            margin-bottom: 20px;
            border-bottom: 2px solid #ddd;
        }
        .tab-button {
            background: #e4e6eb;
            border: none;
            padding: 12px 24px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            border-radius: 8px 8px 0 0;
            transition: 0.2s;
            color: #1e1e2f;
        }
        .tab-button.active {
            background: #ffffff;
            border-bottom: 3px solid #4361ee;
            color: #4361ee;
            font-weight: 600;
        }
        .tab-content {
            background: white;
            border-radius: 0 8px 8px 8px;
            padding: 30px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            min-height: 500px;
        }
        .tab-pane {
            display: none;
        }
        .tab-pane.active {
            display: block;
        }
        h2 {
            color: #1e1e2f;
            margin-bottom: 20px;
            font-weight: 500;
            border-left: 5px solid #4361ee;
            padding-left: 15px;
        }
        .form-expander {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 30px;
            border: 1px solid #e0e0e0;
        }
        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }
        .form-group {
            display: flex;
            flex-direction: column;
        }
        label {
            font-weight: 600;
            margin-bottom: 5px;
            color: #2c3e50;
        }
        input, select, textarea {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 14px;
        }
        button {
            background: #4361ee;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
            transition: 0.2s;
            font-size: 16px;
            margin-right: 10px;
        }
        button:hover {
            background: #3046c0;
        }
        .btn-small {
            padding: 5px 10px;
            font-size: 14px;
            background: #dc3545;
        }
        .btn-small:hover {
            background: #bb2d3b;
        }
        .btn-secondary {
            background: #6c757d;
        }
        .btn-secondary:hover {
            background: #5a6268;
        }
        .btn-success {
            background: #28a745;
        }
        .btn-success:hover {
            background: #218838;
        }
        .btn-info {
            background: #17a2b8;
        }
        .btn-info:hover {
            background: #138496;
        }
        .quadrantes {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            margin-top: 20px;
        }
        .quadrante {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 15px;
            border-left: 5px solid;
        }
        .quadrante h3 {
            margin-bottom: 15px;
            font-size: 16px;
        }
        .quadrante .atividade-item {
            background: white;
            border-radius: 5px;
            padding: 10px;
            margin-bottom: 10px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
            position: relative;
        }
        .check-label {
            display: flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
        }
        .check-label input {
            width: 18px;
            height: 18px;
        }
        .data-info {
            font-size: 12px;
            color: #666;
        }
        .popover {
            background: #e9ecef;
            border-radius: 5px;
            padding: 10px;
            margin-top: 5px;
            font-size: 13px;
        }
        .delete-btn {
            position: absolute;
            top: 5px;
            right: 5px;
            background: #dc3545;
            color: white;
            border: none;
            border-radius: 3px;
            padding: 2px 6px;
            font-size: 12px;
            cursor: pointer;
        }
        .delete-btn:hover {
            background: #bb2d3b;
        }
        .tarefa-item {
            display: flex;
            align-items: center;
            gap: 15px;
            background: #f8f9fa;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 8px;
            position: relative;
        }
        .tarefa-item .detalhes {
            flex: 1;
        }
        .tarefa-item .data {
            color: #666;
            font-size: 14px;
        }
        .orcamento-item {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
            border: 1px solid #ddd;
        }
        .orcamento-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        .orcamento-header h3 {
            margin: 0;
        }
        .orcamento-header .total {
            font-size: 1.2rem;
            font-weight: 700;
            color: #28a745;
        }
        .itens-table {
            width: 100%;
            border-collapse: collapse;
            margin: 15px 0;
        }
        .itens-table th {
            background: #e9ecef;
            padding: 10px;
            text-align: left;
        }
        .itens-table td {
            padding: 8px;
            border-bottom: 1px solid #ddd;
        }
        .itens-table tr:last-child td {
            border-bottom: none;
        }
        .resumo-card {
            background: #e6f7ff;
            border: 1px solid #91d5ff;
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            margin: 20px 0;
        }
        .resumo-card .valor {
            font-size: 32px;
            font-weight: 700;
            color: #1890ff;
        }
        .chart-container {
            height: 300px;
            margin: 30px 0;
        }
        .filtros-periodo {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            align-items: center;
        }
        .metricas {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
            margin-bottom: 30px;
        }
        .metrica {
            background: #f8f9fa;
            padding: 15px 25px;
            border-radius: 8px;
            text-align: center;
            flex: 1;
            min-width: 120px;
        }
        .metrica .numero {
            font-size: 28px;
            font-weight: 700;
            color: #4361ee;
        }
        .calendario-container {
            margin-bottom: 30px;
            background: #f8f9fa;
            border-radius: 8px;
            padding: 20px;
        }
        .calendario-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        .calendario-header h3 {
            font-size: 1.5rem;
            margin: 0;
        }
        .calendario-header button {
            background: #4361ee;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
        }
        .calendario-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
        }
        .calendario-dia-semana {
            font-weight: 600;
            text-align: center;
            padding: 10px;
            background: #e9ecef;
            border-radius: 5px;
        }
        .calendario-dia {
            background: white;
            border: 1px solid #ddd;
            border-radius: 5px;
            min-height: 80px;
            padding: 5px;
            position: relative;
        }
        .calendario-dia.outro-mes {
            background: #f1f3f5;
            color: #aaa;
        }
        .calendario-dia.has-activity {
            background: #cfe8ff;
            border: 2px solid #4361ee;
        }
        .calendario-dia .dia-numero {
            font-weight: 600;
            margin-bottom: 5px;
        }
        .calendario-dia .atividades-resumo {
            font-size: 11px;
            color: #2c3e50;
        }
        .relato-item {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 15px;
            border-left: 5px solid #17a2b8;
            position: relative;
        }
        .relato-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            flex-wrap: wrap;
        }
        .relato-header h4 {
            margin: 0;
            color: #17a2b8;
        }
        .copy-btn {
            background: #6c757d;
            color: white;
            border: none;
            padding: 4px 10px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
        }
        .copy-btn:hover {
            background: #5a6268;
        }
    </style>
</head>
<body>
<div class="container">
    <h1>📋 Dashboard de Planejamento Pessoal</h1>

    <!-- Barra de backup/restauração -->
    <div class="backup-bar">
        <button onclick="exportarDados()"><i class="fas fa-download"></i> Exportar Dados</button>
        <label for="file-import" class="import-label"><i class="fas fa-upload"></i> Importar Dados</label>
        <input type="file" id="file-import" accept=".json" onchange="importarDados(event)">
        <span style="color: #666; font-size: 14px;">Use para transferir dados entre dispositivos.</span>
    </div>

    <!-- Abas -->
    <div class="tabs" id="tabs">
        <button class="tab-button active" data-tab="tab1">📌 Planejamento (Eisenhower)</button>
        <button class="tab-button" data-tab="tab2">🔄 Tarefas Rotineiras</button>
        <button class="tab-button" data-tab="tab3">📅 Agenda</button>
        <button class="tab-button" data-tab="tab4">📊 Relatório</button>
        <button class="tab-button" data-tab="tab5">💰 Orçamentos</button>
        <button class="tab-button" data-tab="tab6">📝 Relatos</button>
    </div>

    <div class="tab-content" id="tabContent">
        <!-- Aba 1: Eisenhower -->
        <div class="tab-pane active" id="tab1">
            <h2>Matriz de Eisenhower</h2>
            <div class="form-expander">
                <h3>➕ Adicionar nova atividade</h3>
                <div class="form-grid">
                    <div class="form-group">
                        <label>Título</label>
                        <input type="text" id="eisen-titulo" placeholder="Ex: Preparar apresentação">
                    </div>
                    <div class="form-group">
                        <label>Descrição</label>
                        <input type="text" id="eisen-descricao" placeholder="Detalhes">
                    </div>
                    <div class="form-group">
                        <label>Data</label>
                        <input type="date" id="eisen-data">
                    </div>
                    <div class="form-group">
                        <label>Quadrante</label>
                        <select id="eisen-quadrante">
                            <option value="Urgente e Importante (Fazer agora)">🔴 Urgente e Importante</option>
                            <option value="Importante, mas não Urgente (Agendar)">🟡 Importante, não Urgente</option>
                            <option value="Urgente, mas não Importante (Delegar)">🟠 Urgente, não Importante</option>
                            <option value="Nem Urgente nem Importante (Eliminar)">🔵 Nem Urgente nem Importante</option>
                        </select>
                    </div>
                </div>
                <button onclick="adicionarEisenhower()">Salvar</button>
            </div>
            <div class="quadrantes" id="eisen-quadrantes"></div>
        </div>

        <!-- Aba 2: Tarefas Rotineiras -->
        <div class="tab-pane" id="tab2">
            <h2>Tarefas Rotineiras</h2>
            <div class="form-expander">
                <h3>➕ Adicionar nova tarefa</h3>
                <div class="form-grid">
                    <div class="form-group">
                        <label>Título</label>
                        <input type="text" id="rot-titulo">
                    </div>
                    <div class="form-group">
                        <label>Descrição</label>
                        <input type="text" id="rot-descricao">
                    </div>
                    <div class="form-group">
                        <label>Data</label>
                        <input type="date" id="rot-data">
                    </div>
                </div>
                <button onclick="adicionarRotineira()">Salvar</button>
            </div>
            <div id="rotineiras-lista"></div>
        </div>

        <!-- Aba 3: Agenda com calendário -->
        <div class="tab-pane" id="tab3">
            <h2>Agenda de Atividades</h2>
            <div class="calendario-container">
                <div class="calendario-header">
                    <button onclick="mesAnterior()"><i class="fas fa-chevron-left"></i> Mês anterior</button>
                    <h3 id="mes-ano-titulo"></h3>
                    <button onclick="proximoMes()">Próximo mês <i class="fas fa-chevron-right"></i></button>
                </div>
                <div class="calendario-grid" id="calendario-grid"></div>
            </div>
            <h3>Atividades Pendentes</h3>
            <div id="agenda-lista"></div>
        </div>

        <!-- Aba 4: Relatório -->
        <div class="tab-pane" id="tab4">
            <h2>Relatório de Conclusões</h2>
            <div class="filtros-periodo">
                <label>Agrupar por:</label>
                <select id="periodo-select">
                    <option value="semanal">Semanal</option>
                    <option value="quinzenal">Quinzenal</option>
                    <option value="mensal">Mensal</option>
                </select>
                <button onclick="atualizarGraficoPeriodo()">Atualizar</button>
            </div>
            <div class="chart-container">
                <canvas id="grafico-periodo"></canvas>
            </div>
            <h3>Detalhamento por Mês</h3>
            <div class="filtros-periodo">
                <label>Mês:</label>
                <select id="mes-select"></select>
                <button onclick="atualizarDetalhamentoMensal()">Ver</button>
            </div>
            <div id="detalhamento-mensal"></div>
            <div class="metricas">
                <div class="metrica" id="metric-eisen-total">Total: 0</div>
                <div class="metrica" id="metric-eisen-concluidas">Concluídas: 0</div>
                <div class="metrica" id="metric-eisen-pendentes">Pendentes: 0</div>
            </div>
            <div class="metricas">
                <div class="metrica" id="metric-rot-total">Total: 0</div>
                <div class="metrica" id="metric-rot-concluidas">Concluídas: 0</div>
                <div class="metrica" id="metric-rot-pendentes">Pendentes: 0</div>
            </div>
        </div>

        <!-- Aba 5: Orçamentos -->
        <div class="tab-pane" id="tab5">
            <h2>Orçamentos de Atividades</h2>
            <div class="form-expander" id="novo-orcamento-form">
                <h3>➕ Criar novo orçamento</h3>
                <div class="form-grid">
                    <div class="form-group">
                        <label>Descrição da atividade</label>
                        <input type="text" id="orc-descricao" placeholder="Ex: Viagem para Manaus">
                    </div>
                    <div class="form-group">
                        <label>Data do pedido</label>
                        <input type="date" id="orc-data-pedido">
                    </div>
                    <div class="form-group">
                        <label>Data da atividade</label>
                        <input type="date" id="orc-data-atividade">
                    </div>
                </div>
                <button onclick="iniciarNovoOrcamento()">Iniciar orçamento</button>
            </div>

            <div id="orcamento-em-andamento" style="display: none; background: #fff3cd; border: 1px solid #ffeeba; border-radius: 8px; padding: 20px; margin-bottom: 30px;">
                <h3>📌 Orçamento em andamento</h3>
                <p><strong id="orc-descricao-atual"></strong> (Pedido: <span id="orc-data-pedido-atual"></span> | Atividade: <span id="orc-data-atividade-atual"></span>)</p>
                <div class="form-grid">
                    <div class="form-group">
                        <label>Tipo do item</label>
                        <select id="item-tipo">
                            <option value="Alimentação">Alimentação</option>
                            <option value="Táxi">Táxi</option>
                            <option value="Gasolina">Gasolina</option>
                            <option value="Diesel">Diesel</option>
                            <option value="Diária de barqueiro">Diária de barqueiro</option>
                            <option value="Medicamentos">Medicamentos</option>
                            <option value="Outro">Outro</option>
                        </select>
                    </div>
                    <div class="form-group" id="item-outro-group" style="display:none;">
                        <label>Especifique</label>
                        <input type="text" id="item-outro">
                    </div>
                    <div class="form-group">
                        <label>Valor unitário (R$)</label>
                        <input type="number" id="item-valor" step="0.01" min="0">
                    </div>
                    <div class="form-group">
                        <label>Quantidade</label>
                        <input type="number" id="item-qtd" value="1" min="1">
                    </div>
                </div>
                <button class="btn-success" onclick="adicionarItemAoOrcamento()">➕ Adicionar item</button>
                <button class="btn-info" onclick="finalizarOrcamento()">✅ Finalizar orçamento</button>
                <button class="btn-secondary" onclick="cancelarOrcamento()">❌ Cancelar</button>
                <h4 style="margin-top: 20px;">Itens adicionados:</h4>
                <div id="itens-temporarios-lista"></div>
                <p><strong>Total parcial: R$ <span id="total-parcial">0.00</span></strong></p>
            </div>

            <h3>Orçamentos realizados</h3>
            <div id="orcamentos-lista"></div>
            <div class="resumo-card">
                <div>💰 Total acumulado de todos os orçamentos</div>
                <div class="valor" id="orc-total-geral">R$ 0,00</div>
            </div>
            <div class="chart-container">
                <canvas id="grafico-orcamento"></canvas>
            </div>
        </div>

        <!-- Aba 6: Relatos (MODIFICADA) -->
        <div class="tab-pane" id="tab6">
            <h2>Relatos de Atividades</h2>
            <div class="form-expander">
                <h3>➕ Adicionar relato</h3>
                <div class="form-grid">
                    <!-- Novo campo: Atividade -->
                    <div class="form-group">
                        <label>Atividade</label>
                        <input type="text" id="rel-atividade" placeholder="Ex: Reunião, Visita, etc.">
                    </div>
                    <div class="form-group">
                        <label>Objetivo</label>
                        <input type="text" id="rel-objetivo">
                    </div>
                    <div class="form-group">
                        <label>Data</label>
                        <input type="date" id="rel-data">
                    </div>
                    <div class="form-group">
                        <label>Povo</label>
                        <input type="text" id="rel-povo">
                    </div>
                    <div class="form-group">
                        <label>Pessoas</label>
                        <input type="number" id="rel-pessoas" min="1">
                    </div>
                    <div class="form-group">
                        <label>Alcançado?</label>
                        <select id="rel-alcancado">
                            <option value="Sim">Sim</option>
                            <option value="Não">Não</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Desafios</label>
                        <input type="text" id="rel-desafios">
                    </div>
                    <div class="form-group">
                        <label>Pontos importantes</label>
                        <input type="text" id="rel-pontos">
                    </div>
                </div>
                <button onclick="adicionarRelato()">Salvar</button>
            </div>
            <div id="relatos-lista"></div>
        </div>
    </div>
</div>

<script>
    // ==================== INICIALIZAÇÃO ====================
    let atividadesEisenhower = [];
    let tarefasRotineiras = [];
    let orcamentos = [];
    let relatosAtividades = [];
    let mesAtual = new Date().getMonth();
    let anoAtual = new Date().getFullYear();
    let orcamentoAtual = null;
    let graficoPeriodo = null;

    function carregarDados() {
        try {
            if (localStorage.getItem('eisenhower')) atividadesEisenhower = JSON.parse(localStorage.getItem('eisenhower'));
            if (localStorage.getItem('rotineiras')) tarefasRotineiras = JSON.parse(localStorage.getItem('rotineiras'));
            if (localStorage.getItem('orcamentos')) orcamentos = JSON.parse(localStorage.getItem('orcamentos'));
            if (localStorage.getItem('relatos')) relatosAtividades = JSON.parse(localStorage.getItem('relatos'));
        } catch (e) { console.error("Erro ao carregar dados", e); }
    }

    function salvarDados() {
        localStorage.setItem('eisenhower', JSON.stringify(atividadesEisenhower));
        localStorage.setItem('rotineiras', JSON.stringify(tarefasRotineiras));
        localStorage.setItem('orcamentos', JSON.stringify(orcamentos));
        localStorage.setItem('relatos', JSON.stringify(relatosAtividades));
    }

    function hoje() {
        const d = new Date();
        return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
    }

    window.addEventListener('load', function() {
        carregarDados();
        const idsData = ['eisen-data', 'rot-data', 'rel-data', 'orc-data-pedido', 'orc-data-atividade'];
        idsData.forEach(id => {
            const el = document.getElementById(id);
            if (el) el.value = hoje();
        });

        try {
            renderizarTudo();
        } catch (e) {
            console.error("Erro na renderização inicial", e);
        }

        configurarAbas();

        const itemTipo = document.getElementById('item-tipo');
        if (itemTipo) {
            itemTipo.addEventListener('change', function() {
                const grupo = document.getElementById('item-outro-group');
                if (grupo) grupo.style.display = this.value === 'Outro' ? 'block' : 'none';
            });
        }
    });

    function configurarAbas() {
        const botoes = document.querySelectorAll('.tab-button');
        botoes.forEach(btn => {
            btn.addEventListener('click', function(e) {
                const tabId = this.getAttribute('data-tab');
                document.querySelectorAll('.tab-button').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                document.querySelectorAll('.tab-pane').forEach(p => p.classList.remove('active'));
                const alvo = document.getElementById(tabId);
                if (alvo) alvo.classList.add('active');

                if (tabId === 'tab3') {
                    renderizarCalendario(mesAtual, anoAtual);
                } else if (tabId === 'tab5') {
                    renderizarOrcamentos();
                }
            });
        });
    }

    function renderizarTudo() {
        renderizarEisenhower();
        renderizarRotineiras();
        renderizarAgenda();
        renderizarRelatorio();
        renderizarOrcamentos();
        renderizarRelatos();
        renderizarCalendario(mesAtual, anoAtual);
    }

    // ==================== EXPORTAR / IMPORTAR ====================
    function exportarDados() {
        const dados = {
            eisenhower: atividadesEisenhower,
            rotineiras: tarefasRotineiras,
            orcamentos: orcamentos,
            relatos: relatosAtividades
        };
        const blob = new Blob([JSON.stringify(dados, null, 2)], { type: 'application/json' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = `dashboard_${hoje()}.json`;
        a.click();
        URL.revokeObjectURL(url);
    }

    function importarDados(event) {
        const file = event.target.files[0];
        if (!file) return;

        const reader = new FileReader();
        reader.onload = function(e) {
            try {
                const dados = JSON.parse(e.target.result);
                if (dados.eisenhower !== undefined) atividadesEisenhower = dados.eisenhower;
                if (dados.rotineiras !== undefined) tarefasRotineiras = dados.rotineiras;
                if (dados.orcamentos !== undefined) orcamentos = dados.orcamentos;
                if (dados.relatos !== undefined) relatosAtividades = dados.relatos;

                salvarDados();
                renderizarTudo();
                alert('Dados importados com sucesso!');
            } catch (err) {
                alert('Erro ao importar: arquivo inválido.');
            }
        };
        reader.readAsText(file);
        event.target.value = '';
    }

    // ==================== EISENHOWER ====================
    function adicionarEisenhower() {
        const titulo = document.getElementById('eisen-titulo').value.trim();
        if (!titulo) return alert('Título é obrigatório');
        const descricao = document.getElementById('eisen-descricao').value;
        const data = document.getElementById('eisen-data').value;
        const quadrante = document.getElementById('eisen-quadrante').value;
        atividadesEisenhower.push({
            id: Date.now() + Math.random(),
            titulo,
            descricao,
            data,
            quadrante,
            concluida: false
        });
        salvarDados();
        renderizarEisenhower();
        renderizarAgenda();
        renderizarRelatorio();
        renderizarCalendario(mesAtual, anoAtual);
        document.getElementById('eisen-titulo').value = '';
        document.getElementById('eisen-descricao').value = '';
        document.getElementById('eisen-data').value = hoje();
    }

    function toggleEisenhower(id) {
        const item = atividadesEisenhower.find(i => i.id == id);
        if (item) {
            item.concluida = !item.concluida;
            salvarDados();
            renderizarEisenhower();
            renderizarAgenda();
            renderizarRelatorio();
            renderizarCalendario(mesAtual, anoAtual);
        }
    }

    function excluirEisenhower(id) {
        if (confirm('Tem certeza que deseja excluir esta atividade?')) {
            atividadesEisenhower = atividadesEisenhower.filter(i => i.id != id);
            salvarDados();
            renderizarEisenhower();
            renderizarAgenda();
            renderizarRelatorio();
            renderizarCalendario(mesAtual, anoAtual);
        }
    }

    function renderizarEisenhower() {
        const container = document.getElementById('eisen-quadrantes');
        if (!container) return;
        const quadrantes = [
            { key: 'Urgente e Importante (Fazer agora)', titulo: '🔴 Urgente e Importante', cor: '#dc3545' },
            { key: 'Importante, mas não Urgente (Agendar)', titulo: '🟡 Importante, não Urgente', cor: '#ffc107' },
            { key: 'Urgente, mas não Importante (Delegar)', titulo: '🟠 Urgente, não Importante', cor: '#fd7e14' },
            { key: 'Nem Urgente nem Importante (Eliminar)', titulo: '🔵 Nem Urgente nem Importante', cor: '#17a2b8' }
        ];
        let html = '';
        quadrantes.forEach(q => {
            const itens = atividadesEisenhower.filter(i => i.quadrante === q.key);
            html += `<div class="quadrante" style="border-left-color: ${q.cor};">`;
            html += `<h3>${q.titulo}</h3>`;
            itens.forEach(item => {
                html += `<div class="atividade-item">`;
                html += `<button class="delete-btn" onclick="excluirEisenhower(${item.id})">🗑️</button>`;
                html += `<label class="check-label">`;
                html += `<input type="checkbox" ${item.concluida ? 'checked' : ''} onchange="toggleEisenhower(${item.id})">`;
                html += `<strong>${item.titulo}</strong>`;
                html += `</label>`;
                html += `<div class="data-info">${formatarData(item.data)}</div>`;
                if (item.descricao) {
                    html += `<div class="popover">📝 ${item.descricao}</div>`;
                }
                html += `</div>`;
            });
            if (itens.length === 0) html += `<p style="color:#999;">Nenhuma atividade</p>`;
            html += `</div>`;
        });
        container.innerHTML = html;
    }

    // ==================== ROTINEIRAS ====================
    function adicionarRotineira() {
        const titulo = document.getElementById('rot-titulo').value.trim();
        if (!titulo) return alert('Título é obrigatório');
        const descricao = document.getElementById('rot-descricao').value;
        const data = document.getElementById('rot-data').value;
        tarefasRotineiras.push({
            id: Date.now() + Math.random(),
            titulo,
            descricao,
            data,
            concluida: false
        });
        salvarDados();
        renderizarRotineiras();
        renderizarAgenda();
        renderizarRelatorio();
        renderizarCalendario(mesAtual, anoAtual);
        document.getElementById('rot-titulo').value = '';
        document.getElementById('rot-descricao').value = '';
        document.getElementById('rot-data').value = hoje();
    }

    function toggleRotineira(id) {
        const item = tarefasRotineiras.find(i => i.id == id);
        if (item) {
            item.concluida = !item.concluida;
            salvarDados();
            renderizarRotineiras();
            renderizarAgenda();
            renderizarRelatorio();
            renderizarCalendario(mesAtual, anoAtual);
        }
    }

    function excluirRotineira(id) {
        if (confirm('Tem certeza que deseja excluir esta tarefa?')) {
            tarefasRotineiras = tarefasRotineiras.filter(i => i.id != id);
            salvarDados();
            renderizarRotineiras();
            renderizarAgenda();
            renderizarRelatorio();
            renderizarCalendario(mesAtual, anoAtual);
        }
    }

    function renderizarRotineiras() {
        const lista = document.getElementById('rotineiras-lista');
        if (!lista) return;
        if (tarefasRotineiras.length === 0) {
            lista.innerHTML = '<p>Nenhuma tarefa cadastrada.</p>';
            return;
        }
        const ordenadas = [...tarefasRotineiras].sort((a,b) => a.data.localeCompare(b.data));
        let html = '';
        ordenadas.forEach(item => {
            html += `<div class="tarefa-item">`;
            html += `<button class="delete-btn" onclick="excluirRotineira(${item.id})">🗑️</button>`;
            html += `<input type="checkbox" ${item.concluida ? 'checked' : ''} onchange="toggleRotineira(${item.id})">`;
            html += `<div class="detalhes"><strong>${item.titulo}</strong>`;
            if (item.descricao) html += `<br><small>${item.descricao}</small>`;
            html += `</div>`;
            html += `<div class="data">${formatarData(item.data)}</div>`;
            html += `</div>`;
        });
        lista.innerHTML = html;
    }

    // ==================== AGENDA ====================
    function renderizarAgenda() {
        const lista = document.getElementById('agenda-lista');
        if (!lista) return;
        const pendentes = [
            ...atividadesEisenhower.filter(i => !i.concluida).map(i => ({ ...i, tipo: 'Planejamento' })),
            ...tarefasRotineiras.filter(i => !i.concluida).map(i => ({ ...i, tipo: 'Rotina' }))
        ].sort((a,b) => a.data.localeCompare(b.data));
        if (pendentes.length === 0) {
            lista.innerHTML = '<p>Nenhuma atividade pendente.</p>';
            return;
        }
        let html = '<table style="width:100%; border-collapse: collapse;"><tr><th>Data</th><th>Tipo</th><th>Título</th><th>Descrição</th></tr>';
        pendentes.forEach(item => {
            html += `<tr><td>${formatarData(item.data)}</td><td>${item.tipo}</td><td>${item.titulo}</td><td>${item.descricao || ''}</td></tr>`;
        });
        html += '</table>';
        lista.innerHTML = html;
    }

    // ==================== CALENDÁRIO ====================
    function renderizarCalendario(mes, ano) {
        const grid = document.getElementById('calendario-grid');
        const titulo = document.getElementById('mes-ano-titulo');
        if (!grid || !titulo) return;
        const meses = ['Janeiro','Fevereiro','Março','Abril','Maio','Junho','Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'];
        titulo.innerText = `${meses[mes]} ${ano}`;
        const diasSemana = ['Dom','Seg','Ter','Qua','Qui','Sex','Sáb'];
        let html = '';
        diasSemana.forEach(dia => html += `<div class="calendario-dia-semana">${dia}</div>`);

        const primeiroDia = new Date(ano, mes, 1);
        const diaSemanaInicio = primeiroDia.getDay();
        const ultimoDia = new Date(ano, mes + 1, 0).getDate();
        const mesAnterior = mes === 0 ? 11 : mes - 1;
        const anoAnterior = mes === 0 ? ano - 1 : ano;
        const ultimoDiaMesAnterior = new Date(anoAnterior, mesAnterior + 1, 0).getDate();
        const atividadesPorData = obterAtividadesPorData();

        let contador = 0;
        for (let i = diaSemanaInicio - 1; i >= 0; i--) {
            const dia = ultimoDiaMesAnterior - i;
            const dataStr = `${anoAnterior}-${String(mesAnterior+1).padStart(2,'0')}-${String(dia).padStart(2,'0')}`;
            const temAtividade = !!atividadesPorData[dataStr];
            html += `<div class="calendario-dia outro-mes ${temAtividade ? 'has-activity' : ''}"><div class="dia-numero">${dia}</div>${temAtividade ? `<div class="atividades-resumo">${atividadesPorData[dataStr]} atividade(s)</div>` : ''}</div>`;
            contador++;
        }
        for (let dia = 1; dia <= ultimoDia; dia++) {
            const dataStr = `${ano}-${String(mes+1).padStart(2,'0')}-${String(dia).padStart(2,'0')}`;
            const temAtividade = !!atividadesPorData[dataStr];
            html += `<div class="calendario-dia ${temAtividade ? 'has-activity' : ''}"><div class="dia-numero">${dia}</div>${temAtividade ? `<div class="atividades-resumo">${atividadesPorData[dataStr]} atividade(s)</div>` : ''}</div>`;
            contador++;
        }
        const totalCelulas = 42;
        const diasRestantes = totalCelulas - contador;
        const proximoMes = mes === 11 ? 0 : mes + 1;
        const anoProximo = mes === 11 ? ano + 1 : ano;
        for (let dia = 1; dia <= diasRestantes; dia++) {
            const dataStr = `${anoProximo}-${String(proximoMes+1).padStart(2,'0')}-${String(dia).padStart(2,'0')}`;
            const temAtividade = !!atividadesPorData[dataStr];
            html += `<div class="calendario-dia outro-mes ${temAtividade ? 'has-activity' : ''}"><div class="dia-numero">${dia}</div>${temAtividade ? `<div class="atividades-resumo">${atividadesPorData[dataStr]} atividade(s)</div>` : ''}</div>`;
        }
        grid.innerHTML = html;
    }

    function obterAtividadesPorData() {
        const todas = [...atividadesEisenhower, ...tarefasRotineiras];
        const mapa = {};
        todas.forEach(item => { if (item.data) mapa[item.data] = (mapa[item.data] || 0) + 1; });
        return mapa;
    }

    function mesAnterior() {
        if (mesAtual === 0) { mesAtual = 11; anoAtual--; } else { mesAtual--; }
        renderizarCalendario(mesAtual, anoAtual);
    }
    function proximoMes() {
        if (mesAtual === 11) { mesAtual = 0; anoAtual++; } else { mesAtual++; }
        renderizarCalendario(mesAtual, anoAtual);
    }

    // ==================== RELATÓRIO ====================
    function renderizarRelatorio() {
        const metricEisenTotal = document.getElementById('metric-eisen-total');
        const metricEisenConcluidas = document.getElementById('metric-eisen-concluidas');
        const metricEisenPendentes = document.getElementById('metric-eisen-pendentes');
        const metricRotTotal = document.getElementById('metric-rot-total');
        const metricRotConcluidas = document.getElementById('metric-rot-concluidas');
        const metricRotPendentes = document.getElementById('metric-rot-pendentes');

        if (metricEisenTotal) {
            const total = atividadesEisenhower.length;
            const concluidas = atividadesEisenhower.filter(i => i.concluida).length;
            metricEisenTotal.innerText = `Total: ${total}`;
            if (metricEisenConcluidas) metricEisenConcluidas.innerText = `Concluídas: ${concluidas}`;
            if (metricEisenPendentes) metricEisenPendentes.innerText = `Pendentes: ${total - concluidas}`;
        }
        if (metricRotTotal) {
            const total = tarefasRotineiras.length;
            const concluidas = tarefasRotineiras.filter(i => i.concluida).length;
            metricRotTotal.innerText = `Total: ${total}`;
            if (metricRotConcluidas) metricRotConcluidas.innerText = `Concluídas: ${concluidas}`;
            if (metricRotPendentes) metricRotPendentes.innerText = `Pendentes: ${total - concluidas}`;
        }

        const mesSelect = document.getElementById('mes-select');
        if (mesSelect) {
            const todasConcluidas = [
                ...atividadesEisenhower.filter(i => i.concluida),
                ...tarefasRotineiras.filter(i => i.concluida)
            ];
            const mesesSet = new Set();
            todasConcluidas.forEach(i => { if (i.data) mesesSet.add(i.data.substring(0,7)); });
            const meses = Array.from(mesesSet).sort();
            mesSelect.innerHTML = meses.length ? meses.map(m => `<option value="${m}">${m}</option>`).join('') : '<option>Nenhum mês</option>';
        }
        atualizarGraficoPeriodo();
    }

    function atualizarGraficoPeriodo() {
        const canvas = document.getElementById('grafico-periodo');
        if (!canvas) return;
        const periodo = document.getElementById('periodo-select').value;
        const todasConcluidas = [
            ...atividadesEisenhower.filter(i => i.concluida && i.data),
            ...tarefasRotineiras.filter(i => i.concluida && i.data)
        ].map(i => i.data);
        if (todasConcluidas.length === 0) {
            if (graficoPeriodo) graficoPeriodo.destroy();
            return;
        }
        const grupos = {};
        todasConcluidas.forEach(dataStr => {
            const data = new Date(dataStr + 'T12:00:00');
            let chave;
            if (periodo === 'semanal') {
                const semana = getWeekNumber(data);
                chave = `${data.getFullYear()}-S${semana}`;
            } else if (periodo === 'quinzenal') {
                const quinzena = Math.floor((data.getDate()-1)/15)+1;
                chave = `${data.getFullYear()}-${data.getMonth()+1}-Q${quinzena}`;
            } else {
                chave = `${data.getFullYear()}-${data.getMonth()+1}`;
            }
            grupos[chave] = (grupos[chave] || 0) + 1;
        });
        const labels = Object.keys(grupos).sort();
        const valores = labels.map(l => grupos[l]);
        const ctx = canvas.getContext('2d');
        if (graficoPeriodo) graficoPeriodo.destroy();
        graficoPeriodo = new Chart(ctx, {
            type: 'bar',
            data: { labels, datasets: [{ label: 'Concluídas', data: valores, backgroundColor: '#4361ee' }] },
            options: { responsive: true, maintainAspectRatio: false }
        });
    }

    function getWeekNumber(d) {
        d = new Date(Date.UTC(d.getFullYear(), d.getMonth(), d.getDate()));
        d.setUTCDate(d.getUTCDate() + 4 - (d.getUTCDay()||7));
        const yearStart = new Date(Date.UTC(d.getUTCFullYear(),0,1));
        return Math.ceil(( ( (d - yearStart) / 86400000) + 1)/7);
    }

    function atualizarDetalhamentoMensal() {
        const mesAno = document.getElementById('mes-select').value;
        if (!mesAno) return;
        const [ano, mes] = mesAno.split('-');
        const concluidas = [
            ...atividadesEisenhower.filter(i => i.concluida && i.data),
            ...tarefasRotineiras.filter(i => i.concluida && i.data)
        ].filter(i => {
            const [a, m] = i.data.split('-');
            return a === ano && m === mes;
        });
        let html = `<p>Total no mês: ${concluidas.length}</p>`;
        if (concluidas.length > 0) {
            html += '<table><tr><th>Data</th><th>Tipo</th><th>Título</th></tr>';
            concluidas.sort((a,b) => a.data.localeCompare(b.data)).forEach(i => {
                html += `<tr><td>${formatarData(i.data)}</td><td>${i.tipo ? i.tipo : (i.quadrante ? 'Planejamento' : 'Rotina')}</td><td>${i.titulo}</td></tr>`;
            });
            html += '</table>';
        }
        document.getElementById('detalhamento-mensal').innerHTML = html;
    }

    // ==================== ORÇAMENTOS ====================
    function iniciarNovoOrcamento() {
        const descricao = document.getElementById('orc-descricao').value.trim();
        const dataPedido = document.getElementById('orc-data-pedido').value;
        const dataAtividade = document.getElementById('orc-data-atividade').value;
        if (!descricao) return alert('Descrição da atividade é obrigatória');
        if (!dataPedido || !dataAtividade) return alert('Informe as datas');
        orcamentoAtual = { descricao, dataPedido, dataAtividade, itens: [] };
        document.getElementById('orc-descricao-atual').innerText = descricao;
        document.getElementById('orc-data-pedido-atual').innerText = formatarData(dataPedido);
        document.getElementById('orc-data-atividade-atual').innerText = formatarData(dataAtividade);
        document.getElementById('orcamento-em-andamento').style.display = 'block';
        document.getElementById('novo-orcamento-form').style.display = 'none';
        atualizarItensTemporarios();
    }

    function adicionarItemAoOrcamento() {
        if (!orcamentoAtual) return alert('Nenhum orçamento em andamento');
        let tipo = document.getElementById('item-tipo').value;
        if (tipo === 'Outro') {
            tipo = document.getElementById('item-outro').value.trim();
            if (!tipo) return alert('Especifique o tipo do item');
        }
        const valor = parseFloat(document.getElementById('item-valor').value);
        if (isNaN(valor) || valor <= 0) return alert('Valor unitário inválido');
        const qtd = parseInt(document.getElementById('item-qtd').value) || 1;
        orcamentoAtual.itens.push({ tipo, valorUnitario: valor, quantidade: qtd, total: valor * qtd });
        document.getElementById('item-valor').value = '';
        document.getElementById('item-qtd').value = 1;
        document.getElementById('item-outro').value = '';
        atualizarItensTemporarios();
    }

    function atualizarItensTemporarios() {
        const lista = document.getElementById('itens-temporarios-lista');
        if (!lista) return;
        if (orcamentoAtual.itens.length === 0) {
            lista.innerHTML = '<p>Nenhum item adicionado.</p>';
        } else {
            let html = '<table class="itens-table"><tr><th>Tipo</th><th>Valor unit.</th><th>Qtd</th><th>Total</th></tr>';
            orcamentoAtual.itens.forEach(item => {
                html += `<tr><td>${item.tipo}</td><td>R$ ${item.valorUnitario.toFixed(2)}</td><td>${item.quantidade}</td><td>R$ ${item.total.toFixed(2)}</td></tr>`;
            });
            html += '</table>';
            lista.innerHTML = html;
        }
        const totalParcial = orcamentoAtual.itens.reduce((acc, i) => acc + i.total, 0);
        document.getElementById('total-parcial').innerText = totalParcial.toFixed(2);
    }

    function finalizarOrcamento() {
        if (!orcamentoAtual) return;
        if (orcamentoAtual.itens.length === 0 && !confirm('Nenhum item adicionado. Deseja finalizar mesmo assim?')) return;
        const novoOrcamento = {
            id: Date.now() + Math.random(),
            descricao: orcamentoAtual.descricao,
            dataPedido: orcamentoAtual.dataPedido,
            dataAtividade: orcamentoAtual.dataAtividade,
            itens: orcamentoAtual.itens
        };
        orcamentos.push(novoOrcamento);
        salvarDados();
        orcamentoAtual = null;
        document.getElementById('orcamento-em-andamento').style.display = 'none';
        document.getElementById('novo-orcamento-form').style.display = 'block';
        renderizarOrcamentos();
    }

    function cancelarOrcamento() {
        orcamentoAtual = null;
        document.getElementById('orcamento-em-andamento').style.display = 'none';
        document.getElementById('novo-orcamento-form').style.display = 'block';
    }

    function removerOrcamento(id) {
        if (confirm('Tem certeza que deseja excluir este orçamento?')) {
            orcamentos = orcamentos.filter(o => o.id != id);
            salvarDados();
            renderizarOrcamentos();
        }
    }

    function renderizarOrcamentos() {
        const lista = document.getElementById('orcamentos-lista');
        if (!lista) return;
        if (orcamentos.length === 0) {
            lista.innerHTML = '<p>Nenhum orçamento cadastrado.</p>';
        } else {
            const ordenados = [...orcamentos].sort((a, b) => b.dataPedido.localeCompare(a.dataPedido));
            let html = '';
            ordenados.forEach(orc => {
                const totalOrc = orc.itens.reduce((acc, i) => acc + i.total, 0);
                html += `<div class="orcamento-item">`;
                html += `<div class="orcamento-header"><h3>${orc.descricao}</h3><span class="total">Total: R$ ${totalOrc.toFixed(2)}</span>`;
                html += `<button class="btn-small" onclick="removerOrcamento(${orc.id})">🗑️ Excluir</button></div>`;
                html += `<p><strong>Data do pedido:</strong> ${formatarData(orc.dataPedido)} | <strong>Data da atividade:</strong> ${formatarData(orc.dataAtividade)}</p>`;
                if (orc.itens.length > 0) {
                    html += '<table class="itens-table"><tr><th>Tipo</th><th>Valor unit.</th><th>Qtd</th><th>Total</th></tr>';
                    orc.itens.forEach(item => {
                        html += `<tr><td>${item.tipo}</td><td>R$ ${item.valorUnitario.toFixed(2)}</td><td>${item.quantidade}</td><td>R$ ${item.total.toFixed(2)}</td></tr>`;
                    });
                    html += '</table>';
                } else html += '<p><em>Sem itens</em></p>';
                html += `</div>`;
            });
            lista.innerHTML = html;
        }

        const totalGeral = orcamentos.reduce((acc, orc) => acc + orc.itens.reduce((sub, i) => sub + i.total, 0), 0);
        const totalEl = document.getElementById('orc-total-geral');
        if (totalEl) totalEl.innerText = `R$ ${totalGeral.toFixed(2)}`;

        const ctx = document.getElementById('grafico-orcamento')?.getContext('2d');
        if (ctx) {
            if (window.graficoOrc) window.graficoOrc.destroy();
            if (orcamentos.length > 0) {
                const tipos = {};
                orcamentos.forEach(orc => orc.itens.forEach(item => tipos[item.tipo] = (tipos[item.tipo] || 0) + item.total));
                window.graficoOrc = new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: Object.keys(tipos),
                        datasets: [{ label: 'Total por tipo (R$)', data: Object.values(tipos), backgroundColor: '#28a745' }]
                    },
                    options: { responsive: true, maintainAspectRatio: false }
                });
            }
        }
    }

    // ==================== RELATOS (MODIFICADO) ====================
    function adicionarRelato() {
        const atividade = document.getElementById('rel-atividade').value.trim();
        const objetivo = document.getElementById('rel-objetivo').value.trim();
        if (!atividade) return alert('Campo Atividade é obrigatório');
        if (!objetivo) return alert('Objetivo é obrigatório');
        const data = document.getElementById('rel-data').value;
        const povo = document.getElementById('rel-povo').value;
        const pessoas = document.getElementById('rel-pessoas').value;
        const alcancado = document.getElementById('rel-alcancado').value;
        const desafios = document.getElementById('rel-desafios').value;
        const pontos = document.getElementById('rel-pontos').value;

        relatosAtividades.push({
            id: Date.now() + Math.random(),
            atividade,
            objetivo,
            data,
            povo,
            pessoas,
            alcancado,
            desafios,
            pontos
        });
        salvarDados();
        renderizarRelatos();
        // Limpar campos
        document.getElementById('rel-atividade').value = '';
        document.getElementById('rel-objetivo').value = '';
        document.getElementById('rel-povo').value = '';
        document.getElementById('rel-pessoas').value = '';
        document.getElementById('rel-desafios').value = '';
        document.getElementById('rel-pontos').value = '';
        document.getElementById('rel-data').value = hoje();
    }

    function removerRelato(id) {
        if (confirm('Tem certeza que deseja excluir este relato?')) {
            relatosAtividades = relatosAtividades.filter(i => i.id != id);
            salvarDados();
            renderizarRelatos();
        }
    }

    function copiarRelato(id) {
        const relato = relatosAtividades.find(i => i.id == id);
        if (!relato) return;

        const texto = `Atividade: ${relato.atividade}
Objetivo: ${relato.objetivo}
Data: ${formatarData(relato.data)}
Povo: ${relato.povo}
Pessoas: ${relato.pessoas}
Alcançado: ${relato.alcancado}
Desafios: ${relato.desafios || ''}
Pontos importantes: ${relato.pontos || ''}`;

        navigator.clipboard.writeText(texto).then(() => {
            alert('Relato copiado para a área de transferência!');
        }).catch(() => {
            alert('Erro ao copiar. Tente novamente.');
        });
    }

    function renderizarRelatos() {
        const lista = document.getElementById('relatos-lista');
        if (!lista) return;
        if (relatosAtividades.length === 0) {
            lista.innerHTML = '<p>Nenhum relato cadastrado.</p>';
            return;
        }
        let html = '';
        relatosAtividades.sort((a,b) => b.data.localeCompare(a.data)).forEach(rel => {
            html += `<div class="relato-item">`;
            html += `<div class="relato-header">`;
            html += `<h4>${rel.atividade} - ${rel.objetivo}</h4>`;
            html += `<div>`;
            html += `<button class="copy-btn" onclick="copiarRelato(${rel.id})"><i class="fas fa-copy"></i> Copiar</button>`;
            html += `<button class="btn-small" onclick="removerRelato(${rel.id})" style="margin-left:5px;">🗑️</button>`;
            html += `</div>`;
            html += `</div>`;
            html += `<p><strong>Data:</strong> ${formatarData(rel.data)} | <strong>Povo:</strong> ${rel.povo} | <strong>Pessoas:</strong> ${rel.pessoas}</p>`;
            html += `<p><strong>Alcançado:</strong> ${rel.alcancado}</p>`;
            if (rel.desafios) html += `<p><strong>Desafios:</strong> ${rel.desafios}</p>`;
            if (rel.pontos) html += `<p><strong>Pontos importantes:</strong> ${rel.pontos}</p>`;
            html += `</div>`;
        });
        lista.innerHTML = html;
    }

    // Utilitário de formatação de data
    function formatarData(dataStr) {
        if (!dataStr) return '';
        const [ano, mes, dia] = dataStr.split('-');
        return `${dia}/${mes}/${ano}`;
    }
</script>
</body>
</html>
