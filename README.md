# Gerador_loterias

<!doctype html>
<html lang="pt-BR">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#2c3e50">
  <title>Gerador de Loterias</title>
  <link rel="manifest" href="manifest.json">
  <link rel="apple-touch-icon" href="icons/icon-192x192.png">
  <style>
        /* Estilos CSS */
        body {
            font-family: 'Inter', sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #ecf0f1;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            box-sizing: border-box;
        }

        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 20px;
            font-size: 2.2em; /* Um pouco menor para caber mais */
        }

        .button-group {
            display: flex;
            flex-direction: column; /* Botões um abaixo do outro em telas menores */
            gap: 12px; /* Espaço entre os botões */
            width: 100%;
            max-width: 350px; /* Largura máxima para os botões */
            margin-bottom: 25px; /* Espaço antes dos resultados */
        }

        button {
            padding: 14px 22px;
            font-size: 1.1em;
            border: none;
            border-radius: 8px; /* Bordas um pouco menos arredondadas */
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.1s ease;
            color: white;
            font-weight: bold;
            box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        button:active {
            transform: translateY(2px);
        }

        /* Cores para os botões das diferentes loterias */
        button.mega-sena { background-color: #27ae60; }
        button.mega-sena:hover { background-color: #2ecc71; }

        button.lotofacil { background-color: #9b59b6; } /* Roxo */
        button.lotofacil:hover { background-color: #a76fc3; }

        button.quina { background-color: #f1c40f; color: #333; } /* Amarelo */
        button.quina:hover { background-color: #f4d03f; }

        button.lotomania { background-color: #e67e22; } /* Laranja */
        button.lotomania:hover { background-color: #eb984e; }

        button.dupla-sena { background-color: #3498db; } /* Azul */
        button.dupla-sena:hover { background-color: #5dade2; }

        button.milionario { background-color: #e74c3c; } /* Vermelho forte */
        button.milionario:hover { background-color: #ec7063; }

        button.loteca { background-color: #1abc9c; } /* Verde água */
        button.loteca:hover { background-color: #48c9b0; }

        .resultado {
            margin-bottom: 15px; /* Espaço entre os resultados */
            padding: 18px;
            font-size: 1.3em;
            font-weight: bold;
            color: #34495e;
            background-color: #ffffff;
            border-radius: 8px;
            text-align: center;
            width: 100%;
            max-width: 450px;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
            word-wrap: break-word;
            line-height: 1.4; /* Espaçamento entre as linhas */
        }

        .resultado span {
            display: block; /* Cada parte do resultado em uma nova linha para Loteca/Dupla Sena */
            margin-top: 5px;
        }

        .resultado span:first-child {
            margin-top: 0;
        }

        /* Media queries para telas maiores (tablets e desktops) */
        @media (min-width: 600px) {
            h1 {
                font-size: 3em;
            }

            .button-group {
                flex-direction: row; /* Botões lado a lado */
                flex-wrap: wrap; /* Quebra para a próxima linha se não couber */
                justify-content: center; /* Centraliza os botões */
                gap: 15px;
                max-width: 700px; /* Mais espaço para os botões */
            }

            button {
                flex: 0 0 auto; /* Não cresce, não encolhe, largura automática */
                width: 180px; /* Largura fixa para os botões em telas maiores */
                font-size: 1.2em;
            }

            .resultado {
                font-size: 1.5em;
                max-width: 600px;
            }
        }
    </style>
 </head>
 <body>
  <h1>Gerador de Loterias</h1>
  <div class="button-group">
   <button onclick="gerarMegaSena()" class="mega-sena">Gerar Mega-Sena</button> <button onclick="gerarLotofacil()" class="lotofacil">Gerar Lotofácil</button> <button onclick="gerarQuina()" class="quina">Gerar Quina</button> <button onclick="gerarLotomania()" class="lotomania">Gerar Lotomania</button> <button onclick="gerarDuplaSena()" class="dupla-sena">Gerar Dupla Sena</button> <button onclick="gerarMilionario()" class="milionario">Gerar +Milionária</button> <button onclick="gerarLoteca()" class="loteca">Gerar Loteca</button>
  </div>
  <div id="megaSenaResultado" class="resultado">
   Mega-Sena: Aguardando...
  </div>
  <div id="lotofacilResultado" class="resultado">
   Lotofácil: Aguardando...
  </div>
  <div id="quinaResultado" class="resultado">
   Quina: Aguardando...
  </div>
  <div id="lotomaniaResultado" class="resultado">
   Lotomania: Aguardando...
  </div>
  <div id="duplaSenaResultado" class="resultado">
   Dupla Sena: Aguardando...
  </div>
  <div id="milionarioResultado" class="resultado">
   +Milionária: Aguardando...
  </div>
  <div id="lotecaResultado" class="resultado">
   Loteca: Aguardando...
  </div>
  <script>
        // Função universal para gerar números únicos e ordenados
        function gerarNumerosUnicos(qtd, min, max) {
            let numeros = new Set();
            while (numeros.size < qtd) {
                let n = Math.floor(Math.random() * (max - min + 1)) + min;
                numeros.add(n);
            }
            return Array.from(numeros).sort((a, b) => a - b);
        }

        // 1. Gerador Mega-Sena (6 números de 1 a 60)
        function gerarMegaSena() {
            const numeros = gerarNumerosUnicos(6, 1, 60);
            document.getElementById('megaSenaResultado').textContent =
                'Mega-Sena: ' + numeros.join(', ');
        }

        // 2. Gerador Lotofácil (15 números de 1 a 25)
        function gerarLotofacil() {
            const numeros = gerarNumerosUnicos(15, 1, 25);
            document.getElementById('lotofacilResultado').textContent =
                'Lotofácil: ' + numeros.join(', ');
        }

        // 3. Gerador Quina (5 números de 1 a 80)
        function gerarQuina() {
            const numeros = gerarNumerosUnicos(5, 1, 80);
            document.getElementById('quinaResultado').textContent =
                'Quina: ' + numeros.join(', ');
        }

        // 4. Gerador Lotomania (50 números de 1 a 100)
        function gerarLotomania() {
            const numeros = gerarNumerosUnicos(50, 1, 100);
            document.getElementById('lotomaniaResultado').textContent =
                'Lotomania: ' + numeros.join(', ');
        }

        // 5. Gerador Dupla Sena (2 sorteios de 6 números de 1 a 50)
        function gerarDuplaSena() {
            const sorteio1 = gerarNumerosUnicos(6, 1, 50);
            const sorteio2 = gerarNumerosUnicos(6, 1, 50);
            document.getElementById('duplaSenaResultado').innerHTML =
                'Dupla Sena:<br><span>1º Sorteio: ' + sorteio1.join(', ') + '</span><br><span>2º Sorteio: ' + sorteio2.join(', ') + '</span>';
        }

        // 6. Gerador +Milionária (6 números de 1 a 50 + 2 trevos de 1 a 6)
        function gerarMilionario() {
            const principais = gerarNumerosUnicos(6, 1, 50);
            const trevos = gerarNumerosUnicos(2, 1, 6);
            document.getElementById('milionarioResultado').textContent =
                '+Milionária: ' + principais.join(', ') + ' + Trevos: ' + trevos.join(', ');
        }

        // 7. Gerador Loteca (14 jogos, resultados 1, X ou 2)
        function gerarLoteca() {
            const resultadosPossiveis = ['1', 'X', '2'];
            let resultadosLoteca = [];
            for (let i = 1; i <= 14; i++) {
                const resultadoAleatorio = resultadosPossiveis[Math.floor(Math.random() * resultadosPossiveis.length)];
                resultadosLoteca.push(`Jogo ${i}: ${resultadoAleatorio}`);
            }
            // Usa innerHTML para poder colocar cada jogo em uma linha separada
            document.getElementById('lotecaResultado').innerHTML = 'Loteca:<br>' + resultadosLoteca.map(r => `<span>${r}</span>`).join('');
        }


        // ---------- REGISTRO DO SERVICE WORKER (PARA PWA) ----------
        // Este código tenta registrar o service worker.
        // Ele só vai funcionar se o arquivo 'service-worker.js' existir na mesma pasta
        // e se o site estiver sendo servido via HTTPS (quando online).
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('service-worker.js')
                    .then(registration => {
                        console.log('Service Worker registrado com sucesso:', registration.scope);
                    })
                    .catch(error => {
                        console.error('Falha no registro do Service Worker:', error);
                    });
            });
        }
    </script>
 </body>
</html>
