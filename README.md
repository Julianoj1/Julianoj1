<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minerador de APH Token</title>
</head>
<body>
    <h1>Simulador de Mineração de APH Token</h1>
    <p>Nome da Moeda: <strong>APH Token</strong></p>
    <p>Total Inicial de Tokens: <span id="totalTokens">5.00000000000000</span></p>
    <p>Quantidade de Mineração: <strong>0.00000000000012 a 0.00000000001823 APH</strong></p>
    <p>Dificuldade: <strong>1</strong></p>
    <p>Liquidez de Mercado: <strong>$300</strong></p>
    <p>Nível de Potência de Mineração: <span id="nivelPotencia">1</span></p>
    <button onclick="iniciarMineracao()">Iniciar Mineração</button>

    <h2>Resultado:</h2>
    <p id="contador">Tempo restante: 2:00 minutos</p>
    <p id="resultado">Clique em "Iniciar Mineração" para começar.</p>
    <p>Total Minerado: <span id="totalMinerado">0.00000000000000</span> APH</p>
    <p>Valor Total em Dólares: <span id="valorEmDolares">$0.00000000000000</span></p>

    <script>
        let totalTokens = 5.00000000000000;  // Total inicial de tokens
        let totalMinerado = 0;  // Total minerado até agora
        let potenciaMineracao = 1;  // Nível de potência de mineração
        const tempoTotal = 120;  // Tempo total em segundos (2 minutos)
        const liquidezMercado = 300;  // Liquidez de mercado em dólares
        let mineracaoIntervalo;
        let tempoDecorrido = 0;  // Tempo decorrido em segundos
        let contagemMineração = 0;  // Contador de quantas vezes a mineração foi iniciada

        function quantidadeMinerada() {
            // Gera um valor aleatório entre 0.00000000000012 e 0.00000000001823
            const baseMin = 0.00000000000012;
            const baseMax = 0.00000000001823;

            // Ajusta a quantidade minerada pela potência de mineração
            const mineracaoAjustadaMin = baseMin * (1 + (potenciaMineracao - 1) / 100);
            const mineracaoAjustadaMax = baseMax * (1 + (potenciaMineracao - 1) / 100);

            return Math.random() * (mineracaoAjustadaMax - mineracaoAjustadaMin) + mineracaoAjustadaMin; // Valor ajustado pela potência
        }

        function atualizarTotalTokens(minerados) {
            totalTokens -= minerados;
            document.getElementById("totalTokens").textContent = totalTokens.toFixed(14); // Formata para 14 casas decimais
        }

        function atualizarTotalMinerado(minerados) {
            totalMinerado += minerados; // Atualiza o total minerado
            document.getElementById("totalMinerado").textContent = totalMinerado.toFixed(14); // Formata para 14 casas decimais
            atualizarValorEmDolares(); // Atualiza o valor em dólares
        }

        function atualizarValorEmDolares() {
            // Calcula o valor de cada token em dólares
            const valorPorToken = liquidezMercado / totalTokens;
            // Calcula o valor total em dólares baseado no total minerado
            const valorTotal = totalMinerado * valorPorToken;
            document.getElementById("valorEmDolares").textContent = `$${valorTotal.toFixed(14)}`; // Formata para 14 casas decimais
        }

        function iniciarMineracao() {
            clearInterval(mineracaoIntervalo);
            tempoDecorrido = 0; // Reinicia o tempo decorrido
            contagemMineração++; // Incrementa a contagem de mineração

            // Aumenta o nível de potência a cada 1500 inícios de mineração
            if (contagemMineração % 1500 === 0) {
                potenciaMineracao += 1; // Aumenta o nível de potência
                document.getElementById("nivelPotencia").textContent = potenciaMineracao; // Atualiza o nível na tela
            }

            // Inicia o contador regressivo
            mineracaoIntervalo = setInterval(() => {
                tempoDecorrido++;

                // Atualiza o contador
                const tempoRestante = tempoTotal - tempoDecorrido;
                const minutos = Math.floor(tempoRestante / 60);
                const segundos = tempoRestante % 60;
                document.getElementById("contador").textContent = `Tempo restante: ${minutos}:${segundos < 10 ? '0' : ''}${segundos} minutos`;

                // Verifica se o tempo de mineração foi alcançado
                if (tempoDecorrido >= tempoTotal) {
                    clearInterval(mineracaoIntervalo);
                    const minerados = quantidadeMinerada(); // Gera a quantidade após 2 minutos
                    atualizarTotalTokens(minerados); // Atualiza o total de tokens
                    atualizarTotalMinerado(minerados); // Atualiza o total minerado
                    document.getElementById("resultado").innerHTML = `Tokens minerados: ${minerados.toFixed(14)} APH<br>Mineração concluída!`;
                }
            }, 1000);
        }
    </script>
</body>
</html>
