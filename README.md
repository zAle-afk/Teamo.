<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Para vos, con canción</title>
  <meta name="description" content="Una carta romántica interactiva que abre una canción especial al presionar el sello." />
  <meta property="og:title" content="Una carta para vos" />
  <meta property="og:description" content="Abrí el sello y escuchá nuestra canción." />
  <meta property="og:type" content="website" />
  <meta name="theme-color" content="#1f1d2b" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&family=Playfair+Display:ital,wght@0,500;0,700;1,600&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg-1:#2b1055; /* morado profundo estilo castillo */
      --bg-2:#501d8a; /* violeta real */
      --bg-3:#1b0f2e; /* noche uva */
      --paper:#fffaf3; /* papel cálido */
      --ink:#1f1d2b;  /* tinta */
      --accent:#c9a227; /* oro viejo */
      --accent-2:#f1d26a; /* oro claro */
      --ring:#ffec99; /* dorado brillo */
      --shadow: 0 20px 60px rgba(0,0,0,.35);
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;display:grid;place-items:center;min-height:100svh;
      font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,"Helvetica Neue",Arial,sans-serif;
      color:var(--ink);
      background: radial-gradient(1200px 900px at 80% -10%, rgba(255,255,255,.06), transparent 60%),
                  radial-gradient(1000px 700px at 10% 110%, rgba(255,255,255,.05), transparent 60%),
                  linear-gradient(120deg,var(--bg-1),var(--bg-2) 45%,var(--bg-3));
      overflow:hidden;
    }
    /* Sutileza de corazones flotando */
    .floaters{position:fixed;inset:0;pointer-events:none;overflow:hidden;}
    .heart{position:absolute;opacity:.12;filter:blur(.2px)}
    .heart svg{display:block;width:22px;height:22px;}
    @keyframes rise{to{transform:translateY(-110vh) scale(.9) rotate(25deg);opacity:0}}

    /* La carta */
    .card{
      width:min(720px,92vw); background:var(--paper);
      background-image: radial-gradient(circle at 10% 10%, rgba(0,0,0,.02) 0 40%, transparent 45%),
                        radial-gradient(circle at 90% 90%, rgba(0,0,0,.02) 0 40%, transparent 45%),
                        linear-gradient(180deg, rgba(255,255,255,.7), rgba(255,255,255,.7)),
                        repeating-linear-gradient( 0deg, transparent 0 22px, rgba(0,0,0,.03) 22px 23px);
      border-radius:28px; padding:48px 40px; box-shadow:var(--shadow);
      position:relative; isolation:isolate;
    }
    .card::before{ /* brillo sutil en el borde */
      content:""; position:absolute; inset:-1px; border-radius:30px; z-index:-1;
      background:linear-gradient(140deg, rgba(255,255,255,.35), rgba(255,255,255,0) 35%, rgba(255,255,255,.15) 60%, rgba(255,255,255,0));
      filter:blur(6px);
    }
    .heading{
      font-family:"Playfair Display", serif; letter-spacing:.4px; line-height:1.15;
      font-size:clamp(28px, 4.5vw, 48px); margin:0 0 20px; color:#16151c;
    }
    .lead{margin:0 0 24px; font-size:clamp(16px, 2.1vw, 18px); color:#2c2a36}
    .signature{margin-top:28px; font-style:italic; opacity:.8}

    /* Sello botón */
    .seal-wrap{display:grid;place-items:center;margin-top:28px}
    .seal{
      --size:96px; width:var(--size); height:var(--size); border-radius:999px; position:relative; cursor:pointer;
      display:grid;place-items:center; text-decoration:none; color:white; font-weight:600;
      background: radial-gradient(60% 60% at 35% 30%, var(--accent-2), var(--accent) 70%);
      border: 3px solid rgba(255,255,255,.35);
      outline: 2px solid rgba(0,0,0,.15);
      transition: transform .25s cubic-bezier(.2,.8,.2,1), box-shadow .25s;
    }
    .seal:hover{transform:translateY(-2px) scale(1.03); box-shadow: 0 12px 30px rgba(217,74,106,.55), inset 0 6px 14px rgba(255,255,255,.28)}
    .seal:active{transform:translateY(0) scale(.98)}
    .seal svg{width:38px;height:38px;}
    .hint{margin-top:12px; font-size:14px; opacity:.7; text-align:center}

    /* Respetar reduce-motion */
    @media (prefers-reduced-motion: reduce){
      .seal{transition:none}
      .heart{animation:none}
    }

    /* Footer mini */
    .footer{margin-top:30px; font-size:12px; opacity:.6; text-align:center}
  </style>
  <!-- Favicon con inicial (C) en dorado/morado -->
  <link rel="icon" type="image/svg+xml" href='data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64"><defs><linearGradient id="g" x1="0" x2="1"><stop stop-color="%23c9a227"/><stop offset="1" stop-color="%23f1d26a"/></linearGradient></defs><rect width="64" height="64" rx="12" fill="%23501d8a"/><text x="50%" y="58%" text-anchor="middle" font-family="Playfair Display, serif" font-size="42" fill="url(%23g)">C</text></svg>' />

  <!-- Portada OG/Share (data URL) -->
  <link rel="preload" as="image" href='data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 630"><defs><linearGradient id="gg" x1="0" x2="1"><stop stop-color="%232b1055"/><stop offset="1" stop-color="%23501d8a"/></linearGradient></defs><rect width="1200" height="630" fill="url(%23gg)"/><circle cx="1000" cy="140" r="14" fill="%23f1d26a"/><g fill="%23f1d26a" opacity="0.14"><text x="80" y="320" font-family="Playfair Display, serif" font-size="120">Una carta para Constanza</text><text x="80" y="420" font-family="Inter, sans-serif" font-size="48">Abrí el sello y escuchá nuestra canción</text></g></svg>' />
  <meta property="og:image" content='data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 630"><defs><linearGradient id="gg" x1="0" x2="1"><stop stop-color="%232b1055"/><stop offset="1" stop-color="%23501d8a"/></linearGradient></defs><rect width="1200" height="630" fill="url(%23gg)"/><circle cx="1000" cy="140" r="14" fill="%23f1d26a"/><g fill="%23f1d26a" opacity="0.9"><text x="80" y="320" font-family="Playfair Display, serif" font-size="120">Para Constanza</text><text x="80" y="420" font-family="Inter, sans-serif" font-size="48">Nuestra canción vive acá</text></g></svg>' />
</head>
<body>
  <!-- corazones flotando -->
  <div class="floaters" id="floaters" aria-hidden="true"></div>

  <main class="card" role="main" aria-describedby="lead">
    <h1 class="heading">Abrí esta carta</h1>
    <p id="lead" class="lead">
      En el pliegue de la noche guardé un secreto: una canción que dice lo que a veces las palabras no alcanzan.
      Si apretás el sello, se abre para vos.
    </p>

    <div class="seal-wrap">
      <a class="seal" id="seal" href="#" aria-label="Abrir poema y canción">
        <svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true" focusable="false">
          <path d="M12 21s-7.4-4.35-9.6-8.28C

    <p class="signature">— Con amor, A.</p>
    <div class="footer">Hecho con cariño y buen gusto ✨</div>
  </main>

  <script>
    // ⚙️ CONFIG: poné acá tu link real
    const SONG_URL = "https://youtu.be/JdgBBQn50yQ?si=61w-YrKGtrF0AuSa"; // enlace de YouTube de Ale

    // corazones suaves ascendiendo
    const floater = document.getElementById('floaters');
    const N = 18; // cantidad discreta para elegancia
    for(let i=0;i<N;i++){
      const d = document.createElement('div');
      d.className = 'heart';
      d.style.left = Math.random()*100 + 'vw';
      d.style.bottom = (-10 - Math.random()*20) + 'vh';
      const delay = (Math.random()*6).toFixed(2)
      const dur = (14 + Math.random()*12).toFixed(2)
      d.style.animation = `rise ${dur}s linear ${delay}s infinite`;
      d.innerHTML = `<svg viewBox='0 0 24 24' fill='${i%3?"#ff9ab3":"#ffd5dd"}'><path d='M12 21s-7.4-4.35-9.6-8.28C.63 8.91 3.03 5 6.8 5c2.02 0 3.38 1.1 4.2 2.24C11.82 6.1 13.18 5 15.2 5 18.97 5 21.37 8.91 21.6 12.72 19.4 16.65 12 21 12 21z'/></svg>`
      floater.appendChild(d)
    }

    // click en sello: micro-animación + redirección
    const seal = document.getElementById('seal');
    seal.addEventListener('click', (e)=>{
      e.preventDefault();
      // estallido de mini corazones
      burst(seal, 16);
      // abrir canción en nueva pestaña con leve demora para permitir el efecto
      setTimeout(()=>{
        window.open(SONG_URL, '_blank', 'noopener,noreferrer');
      }, 320);
    });

    function burst(node, count){
      const rect = node.getBoundingClientRect();
      for(let i=0;i<count;i++){
        const s = document.createElement('span');
        s.style.position='fixed';
        s.style.left = (rect.left + rect.width/2) + 'px';
        s.style.top  = (rect.top + rect.height/2) + 'px';
        const size = 6 + Math.random()*10; s.style.width = s.style.height = size+'px';
        s.style.transform = `translate(-50%,-50%) rotate(${Math.random()*360}deg)`;
        s.style.background = i%2? '#ff9ab3' : '#ffd5dd';
        s.style.clipPath = 'path("M12 21s-7.4-4.35-9.6-8.28C.63 8.91 3.03 5 6.8 5c2.02 0 3.38 1.1 4.2 2.24C11.82 6.1 13.18 5 15.2 5 18.97 5 21.37 8.91 21.6 12.72 19.4 16.65 12 21 12 21z")';
        s.style.borderRadius = '6px';
        s.style.pointerEvents = 'none';
        s.style.zIndex = 9999;
        document.body.appendChild(s);
        const dx = (Math.random()-.5)*160;
        const dy = (Math.random()-.8)*220;
        s.animate([
          { transform:`translate(-50%,-50%)`, opacity:1 },
          { transform:`translate(${dx}px, ${dy}px)`, opacity:0 }
        ], { duration: 520 + Math.random()*200, easing:'cubic-bezier(.2,.8,.2,1)' }).onfinish = ()=> s.remove();
      }
    }
  </script>
</body>
</html>
