<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Play BNB — Announcement</title>
  <style>
    /* Basic reset */
    * { box-sizing: border-box; margin: 0; padding: 0; }

    /* Page background - pure black */
    html, body { height: 100%; }
    body {
      background: #000;
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      color: #fff;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      overflow: hidden; /* hide scrollbars caused by animated bubbles */
    }

    /* Bubble container behind the content */
    .bubbles {
      position: fixed;
      inset: 0;
      pointer-events: none; /* allow clicks through */
      z-index: 0;
      overflow: hidden;
    }

    .bubble {
      position: absolute;
      border-radius: 50%;
      background: #ffd700; /* solid gold fill */
      box-shadow: 0 0 6px rgba(255,215,0,0.35), inset 0 0 4px rgba(255,255,255,0.06);
      opacity: 0.95;
      transform: translate3d(0,0,0) scale(1);
      will-change: transform, opacity;
    }

    /* Faster, subtle floating animations */
    @keyframes floatY-fast {
      0%   { transform: translate3d(0, 0, 0) scale(1); opacity: 0.9; }
      50%  { transform: translate3d(0, -10px, 0) scale(1.05); opacity: 1; }
      100% { transform: translate3d(0, 0, 0) scale(1); opacity: 0.9; }
    }
    @keyframes driftX-fast {
      0%   { transform: translate3d(0,0,0); }
      50%  { transform: translate3d(6px,0,0); }
      100% { transform: translate3d(0,0,0); }
    }

    /* Center card */
    .card-wrap {
      position: relative;
      z-index: 2; /* above bubbles */
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px;
    }

    .card {
      width: min(980px, 94%);
      background: #000; /* card black */
      border-radius: 14px;
      padding: 48px 36px;
      box-shadow: 0 10px 40px rgba(0,0,0,0.6), 0 0 0 1px rgba(255,215,0,0.03) inset;
      border: 1px solid rgba(255,215,0,0.02);
      text-align: center;
      color: #ffffff; /* white text */
      backdrop-filter: blur(6px);
    }

    /* Very large headline and subtext */
    .headline {
      font-size: clamp(32px, 7vw, 72px);
      font-weight: 800;
      line-height: 1.02;
      color: #ffffff;
      margin-bottom: 18px;
      letter-spacing: -0.02em;
    }

    .subtext {
      font-size: clamp(16px, 2.6vw, 20px);
      color: rgba(255,255,255,0.95);
      margin-top: 8px;
      max-width: 860px;
      margin-left: auto;
      margin-right: auto;
    }

    .note {
      margin-top: 20px;
      font-size: 14px;
      color: rgba(255,255,255,0.85);
    }

    /* Respect reduced motion preferences */
    @media (prefers-reduced-motion: reduce) {
      .bubble { animation: none !important; transition: none !important; }
    }

    /* Responsive spacing */
    @media (max-width: 520px) {
      .card { padding: 28px 18px; }
      .headline { font-size: 28px; }
    }
  </style>
</head>
<body>
  <!-- animated gold bubbles (small, filled, faster) -->
  <div class="bubbles" id="bubbles"></div>

  <!-- main centered card -->
  <div class="card-wrap">
    <main class="card" role="main" aria-labelledby="headline">
      <h1 id="headline" class="headline">
        Only users who follow our X and Telegram channels will be eligible to win the airdrop.
      </h1>

      <p class="subtext">
        Winners who follow our official X and Telegram pages will receive rewards distributed through the Gaming and Airdrop sections. Make sure to follow both channels to increase your eligibility.
      </p>

      <p class="note">
        Follow our channels for announcements and rules. This is the official eligibility requirement for upcoming airdrops and in-game rewards.
      </p>
    </main>
  </div>

  <script>
    // Create many very small animated gold bubbles, filled circles, a bit faster.
    (function createBubbles() {
      const container = document.getElementById('bubbles');
      const COUNT = 36; // more small fast bubbles but still subtle
      const vw = Math.max(document.documentElement.clientWidth || 0, window.innerWidth || 0);
      const vh = Math.max(document.documentElement.clientHeight || 0, window.innerHeight || 0);

      for (let i = 0; i < COUNT; i++) {
        const b = document.createElement('div');
        b.className = 'bubble';

        // random very small size between 4 and 12 px
        const size = Math.floor(Math.random() * 9) + 4; // 4..12
        b.style.width = size + 'px';
        b.style.height = size + 'px';

        // random position across viewport, avoid placing many exactly at center
        const left = Math.random() * 100;
        const top = Math.random() * 100;

        b.style.left = left + 'vw';
        b.style.top = top + 'vh';

        // faster animation duration and slight variation
        const durY = 1.8 + Math.random() * 2.2; // 1.8s - 4.0s
        const durX = 2.2 + Math.random() * 2.6; // 2.2s - 4.8s
        const delay = Math.random() * 1.2;      // 0 - 1.2s
        b.style.animation = `floatY-fast ${durY}s ease-in-out ${delay}s infinite alternate, driftX-fast ${durX}s ease-in-out ${delay/2}s infinite alternate`;

        // slightly varied opacity for depth
        b.style.opacity = (0.7 + Math.random() * 0.3).toFixed(2);

        // subtle transform scale randomness
        const initialScale = 0.9 + Math.random() * 0.3;
        b.style.transform = `scale(${initialScale})`;

        container.appendChild(b);
      }

      // On resize, nudge positions slightly to remain visually scattered
      let resizeTimer;
      window.addEventListener('resize', function() {
        clearTimeout(resizeTimer);
        resizeTimer = setTimeout(() => {
          const items = container.children;
          for (let i = 0; i < items.length; i++) {
            const el = items[i];
            const left = Math.min(98, Math.max(2, parseFloat(el.style.left || '50') + (Math.random()*6-3)));
            const top = Math.min(98, Math.max(2, parseFloat(el.style.top || '50') + (Math.random()*6-3)));
            el.style.left = left + 'vw';
            el.style.top = top + 'vh';
          }
        }, 150);
      }, { passive: true });
    })();
  </script>
</body>
</html>
