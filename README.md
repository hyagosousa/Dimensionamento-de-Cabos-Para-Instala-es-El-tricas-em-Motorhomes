<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Motorhome Expert | Simulador Elétrico Profissional</title>
    <style>
        :root {
            --bg-dark: #0f172a;
            --card-bg: #1e293b;
            --accent: #10b981;
            --accent-hover: #059669;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border: #334155;
            --danger: #ef4444;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-main);
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: var(--card-bg);
            padding: 35px;
            border-radius: 16px;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.4), 0 8px 10px -6px rgba(0, 0, 0, 0.4);
            border: 1px solid var(--border);
        }

        .header {
            text-align: center;
            border-bottom: 2px solid var(--border);
            padding-bottom: 20px;
            margin-bottom: 30px;
        }

        .header h1 {
            margin: 0;
            font-size: 1.8rem;
            color: var(--text-main);
            letter-spacing: -0.5px;
        }

        .header h1 span {
            color: var(--accent);
        }

        .header p {
            color: var(--text-muted);
            margin-top: 8px;
            font-size: 0.95rem;
        }

        .dev-credit {
            margin-top: 10px;
            font-size: 0.85rem;
            color: var(--accent);
            font-weight: 600;
        }

        .form-grid {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr auto;
            gap: 12px;
            margin-bottom: 25px;
            align-items: end;
        }

        @media(max-width: 850px) {
            .form-grid {
                grid-template-columns: 1fr;
            }
        }

        label {
            font-weight: 600;
            font-size: 0.8rem;
            color: var(--text-muted);
            display: block;
            margin-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        input, select {
            width: 100%;
            padding: 11px 14px;
            background-color: #0b0f19;
            border: 1px solid var(--border);
            color: var(--text-main);
            border-radius: 8px;
            box-sizing: border-box;
            font-size: 0.95rem;
            outline: none;
            transition: border-color 0.2s;
        }

        input:focus, select:focus {
            border-color: var(--accent);
        }

        button {
            background-color: var(--accent);
            color: #0f172a;
            border: none;
            padding: 11px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 700;
            font-size: 0.95rem;
            transition: background 0.2s, transform 0.1s;
        }

        button:hover {
            background-color: var(--accent-hover);
        }

        button:active {
            transform: scale(0.98);
        }

        .btn-remover {
            background-color: rgba(239, 68, 68, 0.15);
            color: var(--danger);
            border: 1px solid rgba(239, 68, 68, 0.3);
            padding: 8px 12px;
            font-size: 0.85rem;
        }

        .btn-remover:hover {
            background-color: var(--danger);
            color: white;
        }

        .acoes-topo {
            display: flex;
            justify-content: flex-end;
            margin-bottom: 15px;
        }

        .btn-exportar {
            background-color: #3b82f6;
            color: white;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn-exportar:hover {
            background-color: #2563eb;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            overflow: hidden;
            border-radius: 8px;
        }

        th, td {
            border: 1px solid var(--border);
            padding: 14px;
            text-align: center;
            font-size: 0.9rem;
        }

        th {
            background-color: #111827;
            color: var(--text-muted);
            text-transform: uppercase;
            font-size: 0.75rem;
            letter-spacing: 0.8px;
        }

        tr:nth-child(even) {
            background-color: rgba(255, 255, 255, 0.01);
        }

        .resumo-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
            margin-top: 35px;
        }

        .card-resumo {
            background: #111827;
            padding: 20px;
            border-radius: 10px;
            border: 1px solid var(--border);
            border-left: 4px solid var(--accent);
        }

        .card-resumo h3 {
            margin: 0 0 8px 0;
            font-size: 0.8rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .card-resumo p {
            margin: 0;
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--text-main);
        }

        .footer-aviso {
            margin-top: 35px;
            padding-top: 20px;
            border-top: 1px solid var(--border);
            text-align: center;
            color: var(--text-muted);
            font-size: 0.85rem;
        }

        .footer-aviso a {
            color: var(--accent);
            text-decoration: none;
            font-weight: bold;
        }

        .footer-aviso a:hover {
            text-decoration: underline;
        }

        @media print {
            body { background: white; color: black; padding: 0; }
            .container { box-shadow: none; border: none; background: white; width: 100%; max-width: 100%; padding: 0; }
            .form-grid, .acoes-topo, .btn-remover, th:last-child, td:last-child { display: none !important; }
            .header h1 { color: #000; }
            .header h1 span { color: #059669; }
            .card-resumo { background: #f8fafc; border: 1px solid #cbd5e1; border-left: 4px solid #059669; }
            .card-resumo h3 { color: #475569; }
            .card-resumo p { color: #0f172a; }
            th { background-color: #e2e8f0; color: #334155; }
            th, td { border-color: #cbd5e1; color: #1e293b; }
            .footer-aviso { color: #334155; }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>Motorhome Expert <span>| Elétrica</span></h1>
        <p>Simulador Profissional de Dimensionamento de Cargas e Fiação</p>
        <div class="dev-credit">Desenvolvido por Hyago Alves de Sousa</div>
    </div>

    <div class="form-grid">
        <div>
            <label>Equipamento</label>
            <input type="text" id="nomeEq" placeholder="Ex: Geladeira 12V">
        </div>
        <div>
            <label>Potência (W)</label>
            <input type="number" id="potenciaEq" placeholder="Ex: 60">
        </div>
        <div>
            <label>Tensão</label>
            <select id="tensaoEq">
                <option value="12">12V (CC)</option>
                <option value="24">24V (CC)</option>
                <option value="127">127V (CA)</option>
                <option value="220">220V (CA)</option>
            </select>
        </div>
        <div>
            <label>Distância Ida (m)</label>
            <input type="number" id="distanciaEq" placeholder="Ex: 3" value="3">
        </div>
        <div>
            <label>&nbsp;</label>
            <button onclick="adicionarEquipamento()">+ Adicionar</button>
        </div>
    </div>

    <div class="acoes-topo">
        <button class="btn-exportar" onclick="exportarRelatorio()">🖨️ Exportar Relatório (PDF)</button>
    </div>

    <table id="tabelaEquipamentos">
        <thead>
            <tr>
                <th>Equipamento</th>
                <th>Potência</th>
                <th>Tensão</th>
                <th>Corrente</th>
                <th>Distância</th>
                <th>Cabo Ideal</th>
                <th>Ações</th>
            </tr>
        </thead>
        <tbody>
            <!-- Itens adicionados entram dinamicamente -->
        </tbody>
    </table>

    <div class="resumo-container">
        <div class="card-resumo">
            <h3>Corrente Total (12V)</h3>
            <p id="total12v">0.00 A</p>
        </div>
        <div class="card-resumo" style="border-left-color: #3b82f6;">
            <h3>Total de Equipamentos</h3>
            <p id="totalItens">0</p>
        </div>
    </div>

    <div class="footer-aviso">
        ⚠️ Em caso de dúvidas na instalação, entre em contato pelo Instagram: <a href="https://instagram.com/Hyagueera" target="_blank">@Hyagueera</a>
    </div>
</div>

<script>
    let equipamentos = [];

    function calcularCabo(corrente, distancia, tensao) {
        let fator = (tensao == 12 || tensao == 24) ? 2 : 1;
        let rho = 0.0172;
        let deltaVMax = tensao * 0.03;
        let secaoMinima = (fator * rho * distancia * corrente) / deltaVMax;

        const tabela = [
            {bitola: 1.5, cap: 15},
            {bitola: 2.5, cap: 21},
            {bitola: 4.0, cap: 28},
            {bitola: 6.0, cap: 36},
            {bitola: 10.0, cap: 50},
            {bitola: 16.0, cap: 68},
            {bitola: 25.0, cap: 89},
            {bitolar: 35.0, cap: 111},
            {bitola: 35.0, cap: 111},
            {bitola: 50.0, cap: 134}
        ];

        let escolhida = "50+ mm²";
        for (let item of tabela) {
            if (item.bitola >= secaoMinima && item.cap >= corrente) {
                escolhida = item.bitola + " mm²";
                break;
            }
        }
        return escolhida;
    }

    function adicionarEquipamento() {
        let nome = document.getElementById("nomeEq").value.trim();
        let potencia = parseFloat(document.getElementById("potenciaEq").value);
        let tensao = parseFloat(document.getElementById("tensaoEq").value);
        let distancia = parseFloat(document.getElementById("distanciaEq").value);

        if (!nome || isNaN(potencia) || potencia <= 0 || isNaN(distancia) || distancia <= 0) {
            alert("Por favor, preencha todos os campos corretamente.");
            return;
        }

        let corrente = potencia / tensao;
        let caboRecomendado = calcularCabo(corrente, distancia, tensao);

        equipamentos.push({ nome, potencia, tensao, corrente, distancia, caboRecomendado });

        document.getElementById("nomeEq").value = "";
        document.getElementById("potenciaEq").value = "";
        document.getElementById("nomeEq").focus();

        atualizarTabela();
    }

    function removerEquipamento(index) {
        equipamentos.splice(index, 1);
        atualizarTabela();
    }

    function atualizarTabela() {
        let tbody = document.querySelector("#tabelaEquipamentos tbody");
        tbody.innerHTML = "";

        let somaCorrente12V = 0;

        equipamentos.forEach((eq, index) => {
            let tr = document.createElement("tr");
            tr.innerHTML = `
                <td><strong>${eq.nome}</strong></td>
                <td>${eq.potencia} W</td>
                <td>${eq.tensao} V</td>
                <td><span style="color: var(--accent); font-weight: bold;">${eq.corrente.toFixed(2)} A</span></td>
                <td>${eq.distancia} m</td>
                <td><span style="color: #60a5fa; font-weight: bold;">${eq.caboRecomendado}</span></td>
                <td><button class="btn-remover" onclick="removerEquipamento(${index})">Excluir</button></td>
            `;
            tbody.appendChild(tr);

            if (eq.tensao == 12) {
                somaCorrente12V += eq.corrente;
            }
        });

        document.getElementById("total12v").innerText = somaCorrente12V.toFixed(2) + " A";
        document.getElementById("totalItens").innerText = equipamentos.length;
    }

    function exportarRelatorio() {
        if (equipamentos.length === 0) {
            alert("Adicione pelo menos um equipamento antes de exportar o relatório.");
            return;
        }
        window.print();
    }
</script>

</body>
</html>
