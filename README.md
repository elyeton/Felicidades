<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>¡Felicidades!</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      height: 100%;
      background: black;
      font-family: 'Arial', sans-serif;
      color: white;
    }

    .container {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      background: rgba(0, 0, 0, 0.6);
      padding: 40px;
      border-radius: 25px;
      z-index: 1;
    }

    h1 {
      font-size: 3em;
    }

    p {
      font-size: 1.5em;
      margin-bottom: 30px;
    }

    .buttons a, .buttons button {
      display: inline-block;
      margin: 10px;
      padding: 10px 20px;
      background-color: #00c853;
      color: white;
      text-decoration: none;
      border-radius: 10px;
      font-weight: bold;
      border: none;
      cursor: pointer;
      transition: background 0.3s;
    }

    .buttons a:hover, .buttons button:hover {
      background-color: #009624;
    }

    canvas {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 0;
    }
  </style>
</head>
<body>
  <canvas id="canvas"></canvas>

  <div class="container" id="card">
    <h1 id="titulo">¡Felicidades!</h1>
    <p id="mensaje">¡Eres increíble!</p>
    <div class="buttons" id="share-buttons"></div>
    <div class="buttons">
      <button onclick="descargarImagen()">Descargar imagen</button>
    </div>
    <audio autoplay loop>
      <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
    </audio>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
  <script>
    const nombre = prompt("¿Cómo te llamas?");
    const url = encodeURIComponent(window.location.href);
    let textoCompartir = "¡Entra a esta página de felicitaciones y siéntete especial!";
    if (nombre) {
      document.getElementById("titulo").textContent = `¡Felicidades, ${nombre}!`;
      document.getElementById("mensaje").textContent = "Este momento es solo para ti. ¡Gracias por existir!";
      textoCompartir = `¡${nombre} ha sido felicitado de una forma especial! ¡Mira tú también!`;
    }

    const shareButtons = document.getElementById("share-buttons");
    shareButtons.innerHTML = `
      <a href="https://wa.me/?text=${encodeURIComponent(textoCompartir)}%20${url}" target="_blank">WhatsApp</a>
      <a href="https://www.facebook.com/sharer/sharer.php?u=${url}" target="_blank">Facebook</a>
      <a href="https://twitter.com/intent/tweet?text=${encodeURIComponent(textoCompartir)}&url=${url}" target="_blank">X</a>
    `;

    function descargarImagen() {
      const card = document.getElementById("card");
      html2canvas(card).then(canvas => {
        const link = document.createElement("a");
        link.download = "felicitacion.png";
        link.href = canvas.toDataURL();
        link.click();
      });
    }

    // Fuegos artificiales
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    let fireworks = [];
    let particles = [];

    class Firework {
      constructor() {
        this.x = Math.random() * canvas.width;
        this.y = canvas.height;
        this.targetY = Math.random() * canvas.height / 2;
        this.speed = 5;
        this.burst = false;
      }

      update() {
        this.y -= this.speed;
        if (this.y <= this.targetY && !this.burst) {
          this.burst = true;
          for (let i = 0; i < 100; i++) {
            particles.push(new Particle(this.x, this.y));
          }
        }
      }

      draw() {
        ctx.beginPath();
        ctx.arc(this.x, this.y, 2, 0, Math.PI * 2);
        ctx.fillStyle = 'white';
        ctx.fill();
      }
    }

    class Particle {
      constructor(x, y) {
        this.x = x;
        this.y = y;
        this.radius = Math.random() * 2;
        this.speedX = (Math.random() - 0.5) * 8;
        this.speedY = (Math.random() - 0.5) * 8;
        this.life = 100;
        this.color = `hsl(${Math.random() * 360}, 100%, 60%)`;
      }

      update() {
        this.x += this.speedX;
        this.y += this.speedY;
        this.life -= 1;
      }

      draw() {
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
        ctx.fillStyle = this.color;
        ctx.fill();
      }
    }

    function animate() {
      ctx.fillStyle = 'rgba(0,0,0,0.2)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      if (Math.random() < 0.05) {
        fireworks.push(new Firework());
      }

      fireworks.forEach((f, i) => {
        f.update();
        f.draw();
        if (f.burst) fireworks.splice(i, 1);
      });

      particles.forEach((p, i) => {
        p.update();
        p.draw();
        if (p.life <= 0) particles.splice(i, 1);
      });

      requestAnimationFrame(animate);
    }

    animate();

    window.addEventListener('resize', () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });
  </script>
</body>
</html>
