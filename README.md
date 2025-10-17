# Calculadora-Digital
Calculadora Digital

Projeto "Calculadora Digital" — uma calculadora simples para operações básicas (adição, subtração, multiplicação, divisão).

<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora Digital</title>
  <style>
    body {
      background-color: #222;
      color: #fff;
      font-family: 'Courier New', Courier, monospace;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .calculadora {
      background-color: #000;
      padding: 20px;
      border-radius: 20px;
      box-shadow: 0 0 30px rgb(14, 230, 234);
      width: 320px;
    }
    .visor {
      background-color: #111;
      color: rgb(202, 243, 17);
      font-size: 2.5rem;
      text-align: right;
      padding: 20px;
      border-radius: 10px;
      margin-bottom: 20px;
      font-family: 'Digital-7', monospace;
      border: 2px solid rgb(1, 196, 255);
      overflow-x: auto;
    }
    .botoes {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }
    .botao {
      padding: 20px;
      font-size: 1.2rem;
      background-color: #333;
      border: none;
      border-radius: 12px;
      color: #fff;
      cursor: pointer;
      box-shadow: 0 0 5px rgb(5, 218, 251);
      transition: background-color 0.2s;
    }
    .botao:hover { background-color: #555; }
    .botao.operador { background-color: rgb(0, 217, 255); color: #000; }
    .botao.igual { background-color: #ff0; color: #000; grid-column: span 2; }
    .botao.zero { grid-column: span 2; }
    @font-face {
      font-family: 'Digital-7';
      src: url('https://fonts.cdnfonts.com/s/14614/Digital7.woff') format('woff');
    }
  </style>
</head>
<body>
  <div class="calculadora" role="application" aria-label="Calculadora Digital">
    <div class="visor" id="visor" aria-live="polite">0</div>
    <div class="botoes" role="group" aria-label="Teclado da calculadora">
      <button class="botao" onclick="limpar()">C</button>
      <button class="botao" onclick="voltar()">←</button>
      <button class="botao operador" onclick="adicionarOperador('/')">÷</button>
      <button class="botao operador" onclick="adicionarOperador('*')">×</button>

      <button class="botao" onclick="adicionarNumero('7')">7</button>
      <button class="botao" onclick="adicionarNumero('8')">8</button>
      <button class="botao" onclick="adicionarNumero('9')">9</button>
      <button class="botao operador" onclick="adicionarOperador('-')">−</button>

      <button class="botao" onclick="adicionarNumero('4')">4</button>
      <button class="botao" onclick="adicionarNumero('5')">5</button>
      <button class="botao" onclick="adicionarNumero('6')">6</button>
      <button class="botao operador" onclick="adicionarOperador('+')">+</button>

      <button class="botao" onclick="adicionarNumero('1')">1</button>
      <button class="botao" onclick="adicionarNumero('2')">2</button>
      <button class="botao" onclick="adicionarNumero('3')">3</button>
      <button class="botao igual" onclick="calcular()">=</button>

      <button class="botao zero" onclick="adicionarNumero('0')">0</button>
      <button class="botao" onclick="adicionarPonto()">.</button>
    </div>
  </div>

  <script>
    const visor = document.getElementById('visor');
    let expressao = '';

    function atualizarVisor() {
      visor.textContent = expressao || '0';
    }

    function adicionarNumero(num) {
      expressao += num;
      atualizarVisor();
    }

    function adicionarOperador(op) {
      if (expressao === '') return;
      const ultimo = expressao[expressao.length - 1];
      if ('+-*/'.includes(ultimo)) {
        expressao = expressao.slice(0, -1);
      }
      expressao += op;
      atualizarVisor();
    }

    function adicionarPonto() {
      const partes = expressao.split(/[\+\-\*\/]/);
      const ultimaParte = partes[partes.length - 1];
      if (!ultimaParte.includes('.')) {
        expressao += '.';
        atualizarVisor();
      }
    }

    function limpar() {
      expressao = '';
      atualizarVisor();
    }

    function voltar() {
      expressao = expressao.slice(0, -1);
      atualizarVisor();
    }

    function calcular() {
      // Validação básica: permitir apenas dígitos, operadores, parênteses, espaços e ponto
      if (!/^[0-9+\-*/().\s]+$/.test(expressao) || expressao.trim() === '') {
        expressao = 'Erro';
        atualizarVisor();
        return;
      }

      try {
        // Eval restrito via Function após validação.
        // Nota: para produção substitua por um parser matemático seguro.
        const resultado = Function('"use strict"; return (' + expressao + ')')();
        if (resultado === Infinity || Number.isNaN(resultado)) {
          expressao = 'Erro';
        } else {
          // reduz erros de precisão de ponto flutuante mostrando até 12 dígitos significativos
          expressao = Number.isFinite(resultado)
            ? parseFloat(resultado.toPrecision(12)).toString()
            : 'Erro';
        }
      } catch {
        expressao = 'Erro';
      }
      atualizarVisor();
    }

    // Atalhos de teclado
    document.addEventListener('keydown', (e) => {
      if (/^[0-9]$/.test(e.key)) adicionarNumero(e.key);
      else if (['+', '-', '*', '/','(',')', '.'].includes(e.key)) adicionarOperador(e.key);
      else if (e.key === 'Enter' || e.key === '=') calcular();
      else if (e.key === 'Backspace') voltar();
      else if (e.key.toLowerCase() === 'c') limpar();
    });

    // inicializa
    atualizarVisor();
  </script>
</body>
</html>
