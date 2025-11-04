<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Relatório de Insumos por Prato - Setembro</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: linear-gradient(135deg, #2c3e50, #4a6491);
            color: white;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
        }
        
        .subtitle {
            font-size: 1.1rem;
            opacity: 0.9;
        }
        
        .filters {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .search-box {
            flex: 1;
            min-width: 250px;
        }
        
        .search-box input {
            width: 100%;
            padding: 10px 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1rem;
        }
        
        .prato-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .prato-card {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
            overflow: hidden;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .prato-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .prato-header {
            background: linear-gradient(135deg, #3498db, #2c3e50);
            color: white;
            padding: 15px;
        }
        
        .prato-title {
            font-size: 1.2rem;
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .prato-vendidos {
            font-size: 0.9rem;
            opacity: 0.9;
        }
        
        .prato-content {
            padding: 15px;
        }
        
        .insumo-list {
            list-style-type: none;
        }
        
        .insumo-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid #f0f0f0;
        }
        
        .insumo-item:last-child {
            border-bottom: none;
        }
        
        .insumo-name {
            font-weight: 500;
        }
        
        .insumo-quantity {
            color: #2c3e50;
            font-weight: 600;
        }
        
        .summary {
            background-color: white;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
            margin-bottom: 30px;
        }
        
        .summary h2 {
            margin-bottom: 15px;
            color: #2c3e50;
            border-bottom: 2px solid #3498db;
            padding-bottom: 8px;
        }
        
        .summary-stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }
        
        .stat-card {
            background: linear-gradient(135deg, #3498db, #2c3e50);
            color: white;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
        }
        
        .stat-value {
            font-size: 1.8rem;
            font-weight: 700;
            margin: 10px 0;
        }
        
        .stat-label {
            font-size: 0.9rem;
            opacity: 0.9;
        }
        
        footer {
            text-align: center;
            margin-top: 30px;
            padding: 20px;
            color: #7f8c8d;
            font-size: 0.9rem;
        }
        
        @media (max-width: 768px) {
            .prato-list {
                grid-template-columns: 1fr;
            }
            
            .filters {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Relatório de Insumos por Prato</h1>
            <p class="subtitle">Análise detalhada dos ingredientes utilizados em cada prato - Setembro</p>
        </header>
        
        <div class="filters">
            <div class="search-box">
                <input type="text" id="searchInput" placeholder="Pesquisar prato ou insumo...">
            </div>
        </div>
        
        <div class="summary">
            <h2>Resumo Geral</h2>
            <div class="summary-stats">
                <div class="stat-card">
                    <div class="stat-label">Total de Pratos</div>
                    <div class="stat-value" id="totalPratos">16</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Pratos Vendidos</div>
                    <div class="stat-value" id="totalVendidos">482</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Insumos Utilizados</div>
                    <div class="stat-value" id="totalInsumos">26</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Prato Mais Vendido</div>
                    <div class="stat-value" id="pratoMaisVendido">PRATO DO DIA</div>
                </div>
            </div>
        </div>
        
        <div class="prato-list" id="pratoList">
            <!-- Os cards dos pratos serão inseridos aqui via JavaScript -->
        </div>
        
        <footer>
            <p>Relatório gerado em outubro de 2023 | Dados referentes ao mês de setembro</p>
        </footer>
    </div>

    <script>
        // Dados dos pratos e seus insumos
        const pratosData = [
            {
                nome: "BIFE À CAVALO",
                vendidos: 33,
                insumos: [
                    { nome: "FILE DE PEITO DE FRANGO", quantidade: "0,12 kg" },
                    { nome: "CARNE BOVINA", quantidade: "0,11 kg" },
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ÓLEO DE SOJA", quantidade: "0,10 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" },
                    { nome: "FAROFA TEMPERADA", quantidade: "0,05 kg" },
                    { nome: "MOLHO DE PIMENTA", quantidade: "0,037 un" }
                ]
            },
            {
                nome: "BIFE COM FRITAS",
                vendidos: 48,
                insumos: [
                    { nome: "CARNE BOVINA", quantidade: "0,11 kg" },
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "BATATA P/FRITAR", quantidade: "0,23 kg" },
                    { nome: "ÓLEO DE SOJA", quantidade: "0,10 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" },
                    { nome: "MOLHO DE PIMENTA", quantidade: "0,037 un" }
                ]
            },
            {
                nome: "CALDO DE FRANGO",
                vendidos: 4,
                insumos: [
                    { nome: "FILE DE PEITO DE FRANGO", quantidade: "0,12 kg" },
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ALHO", quantidade: "0,01 kg" }
                ]
            },
            {
                nome: "CALDO DE VACA ATOLADA",
                vendidos: 4,
                insumos: [
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" }
                ]
            },
            {
                nome: "ESPETO DE FRALDINHA",
                vendidos: 8,
                insumos: [
                    { nome: "ESPETO DE CARNE - FRALDINHA", quantidade: "1 un" }
                ]
            },
            {
                nome: "ESPETO DE FRANBACON",
                vendidos: 2,
                insumos: [
                    { nome: "ESPETO DE FRANBACON", quantidade: "1,17 un" }
                ]
            },
            {
                nome: "FEIJOADA EXPRESS",
                vendidos: 3,
                insumos: [
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ÓLEO DE SOJA", quantidade: "0,10 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" }
                ]
            },
            {
                nome: "FILÉ DE FRANGO",
                vendidos: 14,
                insumos: [
                    { nome: "FILE DE PEITO DE FRANGO", quantidade: "0,12 kg" },
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ÓLEO DE SOJA", quantidade: "0,10 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" }
                ]
            },
            {
                nome: "FILÉ SUÍNO",
                vendidos: 9,
                insumos: [
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ÓLEO DE SOJA", quantidade: "0,10 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" }
                ]
            },
            {
                nome: "JANTINHA COMPLETA C/FRALDINHA",
                vendidos: 12,
                insumos: [
                    { nome: "MANDIOCA", quantidade: "0,35 kg" },
                    { nome: "ESPETO DE CARNE - FRALDINHA", quantidade: "1 un" },
                    { nome: "FAROFA TEMPERADA", quantidade: "0,05 kg" }
                ]
            },
            {
                nome: "JANTINHA COMPLETA C/FRANBACON",
                vendidos: 1,
                insumos: [
                    { nome: "MANDIOCA", quantidade: "0,35 kg" },
                    { nome: "ESPETO DE FRANBACON", quantidade: "1,17 un" },
                    { nome: "FAROFA TEMPERADA", quantidade: "0,05 kg" }
                ]
            },
            {
                nome: "JANTINHA SIMPLES C/FRALDINHA",
                vendidos: 7,
                insumos: [
                    { nome: "MANDIOCA", quantidade: "0,35 kg" },
                    { nome: "ESPETO DE CARNE - FRALDINHA", quantidade: "1 un" },
                    { nome: "FAROFA TEMPERADA", quantidade: "0,05 kg" }
                ]
            },
            {
                nome: "JANTINHA SIMPLES C/FRANBACON",
                vendidos: 3,
                insumos: [
                    { nome: "MANDIOCA", quantidade: "0,35 kg" },
                    { nome: "ESPETO DE FRANBACON", quantidade: "1,17 un" },
                    { nome: "FAROFA TEMPERADA", quantidade: "0,05 kg" }
                ]
            },
            {
                nome: "OMELETE",
                vendidos: 1,
                insumos: [
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ALHO", quantidade: "0,01 kg" }
                ]
            },
            {
                nome: "PRATO DO DIA",
                vendidos: 205,
                insumos: [
                    { nome: "FILE DE PEITO DE FRANGO", quantidade: "0,12 kg" },
                    { nome: "SOBRECOXA", quantidade: "0,11 kg" },
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ÓLEO DE SOJA", quantidade: "0,10 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" },
                    { nome: "BACON", quantidade: "0,01 kg" },
                    { nome: "BIFE A ROLE", quantidade: "0,01 kg" },
                    { nome: "ERVA DOCE", quantidade: "0,00 kg" },
                    { nome: "CENOURA", quantidade: "0,033 kg" },
                    { nome: "ABOBRINHA", quantidade: "0,020 kg" },
                    { nome: "PALITO DE DENTE", quantidade: "0,0049 un" },
                    { nome: "MACARRÃO ESPAGUETE", quantidade: "0,029 kg" },
                    { nome: "CARNE MOÍDA", quantidade: "0,0029 kg" },
                    { nome: "CHUCHU", quantidade: "0,0139 kg" },
                    { nome: "ABÓBORA", quantidade: "0,0234 kg" }
                ]
            },
            {
                nome: "STROGONOFF DE FRANGO",
                vendidos: 6,
                insumos: [
                    { nome: "SOBRECOXA", quantidade: "0,11 kg" },
                    { nome: "GÁS DE COZINHA", quantidade: "0,01 un" },
                    { nome: "ÓLEO DE SOJA", quantidade: "0,10 un" },
                    { nome: "ARROZ", quantidade: "0,11 kg" },
                    { nome: "ALHO", quantidade: "0,01 kg" }
                ]
            }
        ];

        // Função para renderizar os cards dos pratos
        function renderPratos(pratos) {
            const pratoList = document.getElementById('pratoList');
            pratoList.innerHTML = '';
            
            pratos.forEach(prato => {
                const pratoCard = document.createElement('div');
                pratoCard.className = 'prato-card';
                
                let insumosHTML = '';
                prato.insumos.forEach(insumo => {
                    insumosHTML += `
                        <li class="insumo-item">
                            <span class="insumo-name">${insumo.nome}</span>
                            <span class="insumo-quantity">${insumo.quantidade}</span>
                        </li>
                    `;
                });
                
                pratoCard.innerHTML = `
                    <div class="prato-header">
                        <div class="prato-title">${prato.nome}</div>
                        <div class="prato-vendidos">${prato.vendidos} unidades vendidas</div>
                    </div>
                    <div class="prato-content">
                        <ul class="insumo-list">
                            ${insumosHTML}
                        </ul>
                    </div>
                `;
                
                pratoList.appendChild(pratoCard);
            });
        }

        // Função para filtrar pratos
        function filterPratos() {
            const searchTerm = document.getElementById('searchInput').value.toLowerCase();
            
            const filteredPratos = pratosData.filter(prato => {
                // Verifica se o nome do prato corresponde
                if (prato.nome.toLowerCase().includes(searchTerm)) {
                    return true;
                }
                
                // Verifica se algum insumo corresponde
                return prato.insumos.some(insumo => 
                    insumo.nome.toLowerCase().includes(searchTerm)
                );
            });
            
            renderPratos(filteredPratos);
        }

        // Inicializar a página
        document.addEventListener('DOMContentLoaded', function() {
            renderPratos(pratosData);
            
            // Configurar o evento de busca
            document.getElementById('searchInput').addEventListener('input', filterPratos);
            
            // Calcular totais para o resumo
            let totalVendidos = 0;
            let pratoMaisVendido = { nome: '', vendidos: 0 };
            let insumosUnicos = new Set();
            
            pratosData.forEach(prato => {
                totalVendidos += prato.vendidos;
                
                if (prato.vendidos > pratoMaisVendido.vendidos) {
                    pratoMaisVendido = { nome: prato.nome, vendidos: prato.vendidos };
                }
                
                prato.insumos.forEach(insumo => {
                    insumosUnicos.add(insumo.nome);
                });
            });
            
            document.getElementById('totalVendidos').textContent = totalVendidos;
            document.getElementById('pratoMaisVendido').textContent = pratoMaisVendido.nome;
            document.getElementById('totalInsumos').textContent = insumosUnicos.size;
        });
    </script>
</body>
</html>
