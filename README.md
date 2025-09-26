# Enginecontrol.github.io
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Controle Motor</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      background-color: #121212;
      color: white;
      font-family: Arial, sans-serif;
      text-align: center;
      margin-top: 20px;
    }
    .status {
      margin-bottom: 20px;
      font-size: 20px;
    }
    .sinaleiros-container {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 40px;
      margin-bottom: 30px;
    }
    .sinaleiros {
      display: flex;
      gap: 20px;
    }
    .sinaleiro {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background-color: #444;
      box-shadow: 0px 0px 8px rgba(0,0,0,0.6);
    }
    .circulo-indicador {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: linear-gradient(to right, white 50%, #444 50%);
      box-shadow: 0px 0px 8px rgba(0,0,0,0.6);
      transform: rotate(0deg);
    }
    .container {
      display: flex;
      justify-content: center;
      gap: 15px;
      align-items: center;
      flex-wrap: wrap;
      max-width: 500px;
      margin: 0 auto;
    }
    button {
      flex: 1;
      min-width: 100px;
      height: 60px;
      font-size: 18px;
      background-color: #444;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      box-shadow: 0px 4px 6px rgba(0,0,0,0.5);
      display: flex;
      justify-content: center;
      align-items: center;
    }
    button:hover {
      background-color: #666;
    }
  </style>
</head>
<body>
  <h1>Controle do Motor</h1>
  <h3 class="status" id="status">PARADO</h3>

  <!-- Sinaleiros + círculo -->
  <div class="sinaleiros-container">
    <div class="sinaleiros">
      <div id="verde" class="sinaleiro"></div>
      <div id="vermelho" class="sinaleiro"></div>
    </div>
    <div id="circulo" class="circulo-indicador"></div>
  </div>

  <!-- Botões -->
  <div class="container">
    <button onclick="mudarEstado('esquerda')">Anti-horário</button>
    <button onclick="mudarEstado('parar')">Parar</button>
    <button onclick="mudarEstado('direita')">Horário</button>
  </div>

  <script>
    let angulo = 0;
    let girando = false;
    let sentido = 0; // 0 parado, 1 horário, -1 anti-horário
    let ultimaAtualizacao = null;
    const circulo = () => document.getElementById('circulo');

    function animar(timestamp) {
      if (!ultimaAtualizacao) ultimaAtualizacao = timestamp;
      let delta = (timestamp - ultimaAtualizacao) / 1000;
      ultimaAtualizacao = timestamp;
      if (girando) {
        angulo += sentido * 180 * delta; // 180 graus por segundo
        angulo %= 360;
        circulo().style.transform = 'rotate(' + angulo + 'deg)';
      }
      requestAnimationFrame(animar);
    }
    requestAnimationFrame(animar);

    function atualizarStatus(txt) {
      document.getElementById('status').innerText = txt;
      let verde = document.getElementById('verde');
      let vermelho = document.getElementById('vermelho');
      if (txt.includes('PARADO')) {
        verde.style.backgroundColor = 'green';
        vermelho.style.backgroundColor = '#444';
        girando = false; sentido = 0;
      } else if (txt.includes('ANTI-HORÁRIO')) {
        verde.style.backgroundColor = '#444';
        vermelho.style.backgroundColor = 'red';
        girando = true; sentido = -1;
      } else if (txt.includes('HORÁRIO')) {
        verde.style.backgroundColor = '#444';
        vermelho.style.backgroundColor = 'red';
        girando = true; sentido = 1;
      }
    }

    // Como não temos ESP32 no GitHub Pages, simulamos a troca de status
    function mudarEstado(comando) {
      if (comando === "parar") atualizarStatus("PARADO");
      else if (comando === "esquerda") atualizarStatus("GIRANDO ANTI-HORÁRIO");
      else if (comando === "direita") atualizarStatus("GIRANDO HORÁRIO");
    }
  </script>
</body>
</html>
