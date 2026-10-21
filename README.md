<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Elétrico para Motorhome</title>
    <style>
        :root {
            --primary-color: #2c3e50;
            --accent-color: #27ae60;
            --bg-color: #f8f9fa;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: #333;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 900px;
            margin: 0 auto;
            background: #fff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        h1 {
            color: var(--primary-color);
            text-align: center;
            margin-bottom: 5px;
        }
        p.subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
        }
        .form-group {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr auto;
            gap: 10px;
            margin-bottom: 15px;
            align-items: end;
        }
        @media(max-width: 768px) {
            .form-group {
                grid-template-columns: 1fr;
            }
        }
        label {
            font-weight: bold;
            font-size: 0.85rem;
            color: #555;
            display: block;
            margin-bottom: 5px;
        }
        input, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
        }
        button {
            background-color: var(--accent-color);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s;
        }
        button:hover {
            background-color: #219653;
        }
        .btn-remover {
            background-color: #e74c3c;
        }
        .btn-remover:hover {
            background-color: #c0392b;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 30px;
        }
        th, td {
            border: 1px solid #dee2e6;
            padding: 12px;
            text-align: center;
        }
        th {
            background-color: var(--primary-color);
            color: white;
        }
        tr:nth-child(even) {
            background-color: #f2f2f2;
        }
        .resumo-box {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 30px;
        }
        .card-resumo {
            background: #eef2f3;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
            border-left: 5px solid var(--primary-color);
        }
        .card-resumo h3 {
            margin: 0 0 10px 0;
            font-size: 0.9rem;
            color: #555;
        }
        .card-resumo p {
            margin: 0;
            font-size: 1.4rem;
            font-weight: bold;
            color: var(--primary-color);
        }
    </style>
</head>
<body>

<div class="container">
    <h1>🚐 Simulador Elétrico para Motorhome</h1>
    <p class="subtitle">Adicione os equipamentos para calcular a corrente total (Amperes) e o dimensionamento dos cabos.</p>

    <div class="form-group">
        <div>
            <label>Nome do Equipamento</label>
            <input type="text" id="nomeEq" placeholder="Ex: Geladeira 12V">
        </div>
        <div>
            <label>Potência (Watts)</label>
            <input type="number" id="potenciaEq" placeholder="Ex: 60">
        </div>
        <div>
            <label>Tensão (Volts)</label>
            <select id="tensaoEq">
                <option value="12">12V (CC)</option>
                <option value="24">24V (CC)</option>
                <option value="127">127V (CA)</option>
                <option value="220">220V (CA)</option>
            </select>
        </div>
        <div>
            <label>Distância Ida (m)</label>
            <input type="number" id="distanciaEq" placeholder="Ex: 4" value="3">
        </div>
        <div>
            <label>&nbsp;</label>
            <button onclick="adicionarEquipamento()">Adicionar</button>
        </div>
    </div>

    <table id="tabelaEquipamentos">
        <thead>
            <tr>
                <th>Equipamento</th>
                <th>Potência (W)</th>
                <th>Tensão (V)</th>
                <th>Corrente (A)</th>
                <th>Distância (m)</th>
                <th>Cabo Recomendado</th>
                <th>Ações</th>
            </tr>
        </thead>
        <tbody>
            <!-- Os itens adicionados aparecerão aqui -->
        </tbody>
    </table>

    <div class="resumo-box">
        <div class="card-resumo">
            <h3>Corrente Total em 12V</h3>
            <p id="total12v">0.0 A</p>
        </div>
        <div class="card-resumo" style="border-left-color: var(--accent-color);">
            <h3>Equipamentos Cadastrados</h3>
            <p id="totalItens">0</p>
        </div>
    </div>
</div>

<script>
    let equipamentos = [];

    function calcularCabo(corrente, distancia, tensao) {
        // Considera ida e volta (2x) se for 12V ou 24V (Corrente Contínua)
        let fator = (tensao == 12 || tensao == 24) ? 2 : 1;
        let rho = 0.0172; // Resistividade do cobre
        let deltaVMax = tensao * 0.03; // Queda máxima aceitável de 3%
        
        // Fórmula de queda de tensão: S = (2 * rho * L * I) / Delta_V
        let secaoMinima = (fator * rho * distancia * corrente) / deltaVMax;

        // Tabela comercial simplificada de capacidade de corrente para cobre
        const tabela = [
            {bitola: 1.5, cap: 15},
            {bitola: 2.5, cap: 21},
            {bitola: 4.0, cap: 28},
            {bitola: 6.0, cap: 36},
            {bitola: 10.0, cap: 50},
            {bitola: 16.0, cap: 68},
            {bitola: 25.0, cap: 89},
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
            alert("Por favor, preencha todos os campos corretamente!");
            return;
        }

        // Fórmula fundamental da eletricidade: I = P / V (Corrente = Potência / Tensão)
        let corrente = potencia / tensao;
        let caboRecomendado = calcularCabo(corrente, distancia, tensao);

        equipamentos.push({
            nome,
            potencia,
            tensao,
            corrente,
            distancia,
            caboRecomendado
        });

        // Limpa campos de entrada
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
                <td>${eq.nome}</td>
                <td>${eq.potencia} W</td>
                <td>${eq.tensao} V</td>
                <td><strong>${eq.corrente.toFixed(2)} A</strong></td>
                <td>${eq.distancia} m</td>
                <td><span style="color: #27ae60; font-weight: bold;">${eq.caboRecomendado}</span></td>
                <td><button class="btn-remover" onclick="removerEquipamento(${index})">Excluir</button></td>
            `;
            tbody.appendChild(tr);

            // Se for sistema 12V, acumula para o total geral do circuito principal
            if (eq.tensao == 12) {
                somaCorrente12V += eq.corrente;
            }
        });

        document.getElementById("total12v").innerText = somaCorrente12V.toFixed(2) + " A";
        document.getElementById("totalItens").innerText = equipamentos.length;
    }
</script>

</body>
</html>
