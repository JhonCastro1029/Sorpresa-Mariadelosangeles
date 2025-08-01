<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Una Sorpresa Romántica</title>

  <!-- Fuentes elegantes -->
  <link href="https://fonts.googleapis.com/css2?family=Lora:wght@400;700&family=Montserrat:wght@400;600&display=swap" rel="stylesheet" />

  <style>
    body {
      margin: 0;
      font-family: 'Lora', serif;
      background: linear-gradient(135deg, #720026 0%, #c93756 100%);
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      text-align: center;
      padding: 20px;
      overflow-x: hidden;
    }

    /* Fondo de pétalos animados */
    .petalos {
      pointer-events: none;
      position: fixed;
      width: 100%;
      height: 100%;
      top: 0; left: 0;
      z-index: 0;
      overflow: hidden;
    }

    .petalo {
      position: absolute;
      width: 15px;
      height: 15px;
      background: #f5c1ca;
      border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
      opacity: 0.8;
      filter: drop-shadow(0 0 3px #9b1c38);
      animation-name: caer;
      animation-timing-function: linear;
      animation-iteration-count: infinite;
    }

    @keyframes caer {
      0% {
        transform: translateY(-20px) rotate(0deg);
        opacity: 0.9;
      }
      100% {
        transform: translateY(110vh) rotate(360deg);
        opacity: 0;
      }
    }

    /* Contenedor principal */
    .carta {
      background: rgba(255 255 255 / 0.15);
      border: 2px solid #9b1c38;
      border-radius: 25px;
      padding: 40px 60px;
      max-width: 600px;
      box-shadow: 0 10px 40px rgba(155, 28, 56, 0.7);
      position: relative;
      z-index: 1;
      user-select: none;
      transition: box-shadow 0.4s ease;
    }

    .carta:hover {
      box-shadow: 0 15px 60px rgba(155, 28, 56, 0.9);
    }

    h2 {
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      font-size: 2.8rem;
      margin-bottom: 20px;
      letter-spacing: 0.06em;
      text-shadow: 2px 2px 6px rgba(0,0,0,0.25);
      color: #ffced7;
    }

    p {
      font-size: 1.25rem;
      margin-bottom: 40px;
      line-height: 1.6;
      font-style: italic;
      color: #ffd8df;
      text-shadow: 1px 1px 3px rgba(0,0,0,0.2);
      font-weight: 500;
    }

    .botones {
      display: flex;
      justify-content: center;
      gap: 36px;
    }

    button {
      background-color: #9b1c38;
      border: none;
      color: #ffe9ee;
      padding: 16px 42px;
      border-radius: 50px;
      font-size: 1.2rem;
      cursor: pointer;
      box-shadow: 0 8px 25px rgba(155, 28, 56, 0.7);
      transition: background-color 0.3s ease, transform 0.25s ease, box-shadow 0.3s ease;
      user-select: none;
      font-weight: 700;
      font-family: 'Montserrat', sans-serif;
      position: relative;
    }

    button:hover {
      background-color: #c93756;
      transform: scale(1.08);
      box-shadow: 0 12px 35px rgba(201, 55, 86, 0.85);
    }

    button:active {
      animation: clickBounce 0.3s;
      box-shadow: 0 6px 18px rgba(201, 55, 86, 0.7);
    }

    @keyframes clickBounce {
      0% { transform: scale(1); }
      50% { transform: scale(0.9); }
      100% { transform: scale(1); }
    }

    .no-button {
      position: relative;
      transition: transform 0.5s ease-in-out;
    }

    .mover-izquierda {
      transform: translateX(-180px);
      opacity: 0;
    }

    #mensajeEntrada {
      font-family: 'Montserrat', sans-serif;
      font-size: 2.4rem;
      color: #ffe6ea;
      cursor: pointer;
      background: rgba(155, 28, 56, 0.6);
      padding: 22px 50px;
      border-radius: 50px;
      box-shadow: 0 12px 40px rgba(155, 28, 56, 0.9);
      max-width: 360px;
      margin-bottom: 50px;
      user-select: none;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      font-weight: 700;
      letter-spacing: 0.04em;
      text-shadow: 2px 2px 8px rgba(0,0,0,0.25);
    }

    #mensajeEntrada:hover {
      transform: scale(1.15);
      box-shadow: 0 16px 50px rgba(155, 28, 56, 1);
    }
  </style>
</head>
<body>

  <div id="mensajeEntrada" onclick="mostrarSorpresa()">
    💖 Haz clic para entrar a mi corazón 💖
  </div>

  <div class="carta" id="carta" style="display:none;">
    <h2>John tiene una sorpresa para ti...</h2>
    <p>¿Quieres saber qué es? Elige con cariño.</p>
    <div class="botones">
      <button onclick="responder(true)">Sí, cuéntame 💌</button>
      <button class="no-button" id="noButton" onclick="responder(false)">No, quizás luego 💭</button>
    </div>
  </div>

  <audio autoplay loop>
    <source src="https://www.bensound.com/bensound-music/bensound-love.mp3" type="audio/mp3" />
    Tu navegador no soporta audio HTML5.
  </audio>

  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.4.0/dist/confetti.browser.min.js"></script>

  <div class="petalos" id="petalos"></div>

  <script>
    // Crear y animar pétalos románticos
    const petalosContenedor = document.getElementById('petalos');
    const coloresPetalos = ['#f5c1ca', '#d94f66', '#e67381', '#ffafbd'];

    function crearPetalo() {
      const petalo = document.createElement('div');
      petalo.classList.add('petalo');
      petalo.style.left = Math.random() * 100 + 'vw';
      petalo.style.animationDuration = (7 + Math.random() * 6) + 's';
      petalo.style.width = (10 + Math.random() * 15) + 'px';
      petalo.style.height = petalo.style.width;
      petalo.style.backgroundColor = coloresPetalos[Math.floor(Math.random() * coloresPetalos.length)];
      petalo.style.opacity = (0.5 + Math.random() * 0.5);
      petalosContenedor.appendChild(petalo);

      // Limpiar después de la animación
      setTimeout(() => {
        petalo.remove();
      }, 14000);
    }

    // Crear pétalos continuamente
    setInterval(crearPetalo, 350);

    // Lógica botones
    const noButton = document.getElementById("noButton");

    function mostrarSorpresa() {
      document.getElementById("mensajeEntrada").style.display = "none";
      document.getElementById("carta").style.display = "block";
    }

    function responder(acepta) {
      const carta = document.getElementById("carta");

      if (acepta) {
        confetti();
        carta.innerHTML = `
          <h2>🎉 ¡Sorpresa! 🎉</h2>
          <p>Este domingo, a las <strong>11:00 AM</strong>, paso a recogerte para un almuerzo muy especial. 💖</p>
          <p>¡Estoy deseando verte! 😍</p>
        `;
      } else {
        noButton.classList.add("mover-izquierda");
        setTimeout(() => {
          carta.innerHTML = `
            <h2>💔 Tal vez en otra ocasión...</h2>
            <p>No te preocupes, ¡habrá otras sorpresas! 🎁</p>
          `;
        }, 500);
      }
    }

    function confetti() {
      setInterval(() => {
        confetti({
          particleCount: 100,
          spread: 70,
          origin: { x: Math.random(), y: Math.random() }
        });
      }, 200);
    }
  </script>
</body>
</html>
