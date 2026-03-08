<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NNB POLYBAT — Tous Corps d'État</title>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300&family=Bebas+Neue&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --gold: #c9a84c;
    --gold-light: #e8c97a;
    --dark: #0a0a0a;
    --dark-2: #111111;
    --dark-3: #1a1a1a;
    --dark-4: #222222;
    --white: #f5f0e8;
    --white-dim: rgba(245,240,232,0.6);
    --white-faint: rgba(245,240,232,0.1);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  html { scroll-behavior: smooth; }
  body { background: var(--dark); color: var(--white); font-family: 'DM Sans', sans-serif; font-weight: 300; overflow-x: hidden; cursor: none; }

  .cursor { position: fixed; width: 8px; height: 8px; background: var(--gold); border-radius: 50%; pointer-events: none; z-index: 9999; transition: transform 0.1s; transform: translate(-50%, -50%); }
  .cursor-ring { position: fixed; width: 36px; height: 36px; border: 1px solid rgba(201,168,76,0.5); border-radius: 50%; pointer-events: none; z-index: 9998; transform: translate(-50%, -50%); transition: all 0.15s ease; }

  nav { position: fixed; top: 0; left: 0; right: 0; z-index: 100; display: flex; justify-content: space-between; align-items: center; padding: 24px 48px; background: linear-gradient(to bottom, rgba(10,10,10,0.95), transparent); backdrop-filter: blur(2px); }
  .nav-logo { font-family: 'Bebas Neue', sans-serif; font-size: 28px; letter-spacing: 4px; color: var(--white); text-decoration: none; }
  .nav-logo span { color: var(--gold); }
  .nav-links { display: flex; gap: 40px; list-style: none; }
  .nav-links a { color: var(--white-dim); text-decoration: none; font-size: 12px; letter-spacing: 2px; text-transform: uppercase; transition: color 0.3s; }
  .nav-links a:hover { color: var(--gold); }
  .nav-cta { background: transparent; border: 1px solid var(--gold); color: var(--gold) !important; padding: 10px 24px; font-size: 11px !important; letter-spacing: 2px; text-transform: uppercase; text-decoration: none; transition: all 0.3s !important; }
  .nav-cta:hover { background: var(--gold) !important; color: var(--dark) !important; }

  .hero { min-height: 100vh; display: grid; grid-template-columns: 1fr 1fr; position: relative; overflow: hidden; }
  .hero-bg { position: absolute; inset: 0; background: radial-gradient(ellipse 80% 60% at 70% 50%, rgba(201,168,76,0.04) 0%, transparent 60%), radial-gradient(ellipse 40% 40% at 20% 80%, rgba(201,168,76,0.03) 0%, transparent 50%); }
  .hero-grid { position: absolute; inset: 0; background-image: linear-gradient(rgba(201,168,76,0.04) 1px, transparent 1px), linear-gradient(90deg, rgba(201,168,76,0.04) 1px, transparent 1px); background-size: 80px 80px; mask-image: radial-gradient(ellipse 80% 80% at 50% 50%, black 30%, transparent 100%); }
  .hero-left { display: flex; flex-direction: column; justify-content: center; padding: 120px 80px 80px 80px; position: relative; z-index: 2; }
  .hero-badge { display: inline-flex; align-items: center; gap: 10px; margin-bottom: 40px; opacity: 0; animation: fadeUp 0.8s ease 0.3s forwards; }
  .hero-badge span { font-size: 10px; letter-spacing: 3px; text-transform: uppercase; color: var(--gold); }
  .hero-badge::before { content: ''; display: block; width: 30px; height: 1px; background: var(--gold); }
  .hero-title { font-family: 'Bebas Neue', sans-serif; font-size: clamp(80px, 10vw, 140px); line-height: 0.9; letter-spacing: -2px; margin-bottom: 8px; opacity: 0; animation: fadeUp 0.8s ease 0.5s forwards; }
  .hero-title .gold { color: var(--gold); }
  .hero-subtitle { font-family: 'Cormorant Garamond', serif; font-style: italic; font-size: 22px; color: var(--white-dim); margin-bottom: 48px; opacity: 0; animation: fadeUp 0.8s ease 0.7s forwards; }
  .hero-stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 2px; margin-bottom: 48px; opacity: 0; animation: fadeUp 0.8s ease 0.9s forwards; }
  .stat { background: var(--dark-3); padding: 20px; border-top: 1px solid rgba(201,168,76,0.3); }
  .stat-num { font-family: 'Bebas Neue', sans-serif; font-size: 36px; color: var(--gold); line-height: 1; }
  .stat-label { font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase; color: var(--white-dim); margin-top: 4px; }
  .hero-phones { display: flex; flex-direction: column; gap: 8px; opacity: 0; animation: fadeUp 0.8s ease 1.1s forwards; }
  .phone-item { display: flex; align-items: center; gap: 16px; }
  .phone-label { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--gold); width: 120px; }
  .phone-num { font-family: 'Bebas Neue', sans-serif; font-size: 24px; letter-spacing: 2px; color: var(--white); }
  .hero-right { position: relative; display: flex; align-items: center; justify-content: center; overflow: hidden; }
  .hero-visual { position: absolute; inset: 0; background: linear-gradient(135deg, var(--dark-3) 0%, var(--dark-2) 100%); display: flex; align-items: center; justify-content: center; overflow: hidden; }
  .hero-monogram { font-family: 'Bebas Neue', sans-serif; font-size: 280px; color: rgba(201,168,76,0.06); line-height: 1; letter-spacing: -10px; user-select: none; animation: float 8s ease-in-out infinite; }
  .hero-overlay-text { position: absolute; bottom: 60px; left: 40px; right: 40px; display: flex; justify-content: space-between; align-items: flex-end; }
  .certif-badge { display: flex; flex-direction: column; gap: 4px; padding: 20px; border: 1px solid rgba(201,168,76,0.2); background: rgba(10,10,10,0.6); backdrop-filter: blur(10px); }
  .certif-badge .certif-title { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--gold); }
  .certif-badge .certif-val { font-size: 14px; color: var(--white); }
  .hero-divider { position: absolute; top: 0; bottom: 0; left: 0; width: 1px; background: linear-gradient(to bottom, transparent, var(--gold), transparent); opacity: 0.3; }

  .banner { background: var(--gold); padding: 16px 0; overflow: hidden; }
  .banner-track { display: flex; gap: 60px; animation: scroll 20s linear infinite; width: max-content; }
  .banner-item { display: flex; align-items: center; gap: 12px; white-space: nowrap; font-size: 12px; letter-spacing: 2px; text-transform: uppercase; color: var(--dark); font-weight: 500; }
  .banner-dot { width: 4px; height: 4px; border-radius: 50%; background: var(--dark); opacity: 0.4; }

  .services { padding: 120px 80px; background: var(--dark-2); position: relative; }
  .section-header { display: grid; grid-template-columns: 1fr 1fr; gap: 60px; margin-bottom: 80px; align-items: end; }
  .section-label { font-size: 10px; letter-spacing: 4px; text-transform: uppercase; color: var(--gold); margin-bottom: 16px; display: flex; align-items: center; gap: 12px; }
  .section-label::before { content: ''; width: 24px; height: 1px; background: var(--gold); }
  .section-title { font-family: 'Bebas Neue', sans-serif; font-size: clamp(48px, 6vw, 80px); line-height: 0.95; letter-spacing: -1px; }
  .section-desc { font-family: 'Cormorant Garamond', serif; font-size: 20px; color: var(--white-dim); line-height: 1.6; align-self: end; }
  .services-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 2px; }
  .service-card { background: var(--dark-3); padding: 40px 28px; position: relative; overflow: hidden; cursor: none; transition: background 0.4s; }
  .service-card::before { content: ''; position: absolute; bottom: 0; left: 0; right: 0; height: 2px; background: var(--gold); transform: scaleX(0); transform-origin: left; transition: transform 0.4s ease; }
  .service-card:hover { background: var(--dark-4); }
  .service-card:hover::before { transform: scaleX(1); }
  .service-num { font-family: 'Bebas Neue', sans-serif; font-size: 48px; color: rgba(201,168,76,0.15); line-height: 1; margin-bottom: 24px; transition: color 0.4s; }
  .service-card:hover .service-num { color: rgba(201,168,76,0.35); }
  .service-icon { font-size: 24px; margin-bottom: 16px; }
  .service-name { font-family: 'Bebas Neue', sans-serif; font-size: 20px; letter-spacing: 1px; margin-bottom: 12px; color: var(--white); }
  .service-items { list-style: none; display: flex; flex-direction: column; gap: 4px; }
  .service-items li { font-size: 11px; color: var(--white-dim); letter-spacing: 0.5px; padding-left: 10px; position: relative; }
  .service-items li::before { content: '—'; position: absolute; left: 0; color: var(--gold); font-size: 8px; top: 1px; }

  .tarifs { padding: 120px 80px; background: var(--dark); }
  .tarifs-intro { display: grid; grid-template-columns: 1fr 1fr; gap: 80px; margin-bottom: 80px; align-items: center; }
  .tarif-tabs { display: flex; gap: 2px; margin-bottom: 40px; flex-wrap: wrap; }
  .tab-btn { background: var(--dark-3); border: none; color: var(--white-dim); padding: 12px 20px; font-family: 'DM Sans', sans-serif; font-size: 11px; letter-spacing: 1.5px; text-transform: uppercase; cursor: pointer; transition: all 0.3s; }
  .tab-btn.active { background: var(--gold); color: var(--dark); }
  .tab-btn:hover:not(.active) { background: var(--dark-4); color: var(--white); }
  .tarif-table { display: none; background: var(--dark-3); border-top: 2px solid var(--gold); }
  .tarif-table.active { display: block; }
  .tarif-cat-title { padding: 20px 28px; background: var(--dark-4); font-family: 'Bebas Neue', sans-serif; font-size: 18px; letter-spacing: 2px; color: var(--gold); border-bottom: 1px solid rgba(201,168,76,0.1); }
  .tarif-row { display: grid; grid-template-columns: 80px 1fr 60px 100px; border-bottom: 1px solid rgba(245,240,232,0.05); transition: background 0.2s; }
  .tarif-row:hover { background: rgba(201,168,76,0.03); }
  .tarif-row > div { padding: 14px 20px; font-size: 12px; display: flex; align-items: center; }
  .tarif-code { color: var(--gold); font-family: 'Bebas Neue', sans-serif; font-size: 13px; letter-spacing: 1px; border-right: 1px solid rgba(245,240,232,0.05); }
  .tarif-desc { color: var(--white-dim); border-right: 1px solid rgba(245,240,232,0.05); }
  .tarif-unit { color: rgba(245,240,232,0.3); font-size: 11px; justify-content: center; border-right: 1px solid rgba(245,240,232,0.05); }
  .tarif-price { color: var(--white); font-weight: 500; justify-content: flex-end; font-family: 'Bebas Neue', sans-serif; font-size: 16px; letter-spacing: 1px; }
  .tarif-price.gold { color: var(--gold); }

  .garanties { padding: 100px 80px; background: var(--dark-2); display: grid; grid-template-columns: 1fr 1fr; gap: 80px; align-items: center; }
  .garanties-left .section-title { margin-bottom: 40px; }
  .garantie-items { display: flex; flex-direction: column; gap: 2px; }
  .garantie-item { display: grid; grid-template-columns: 60px 1fr; background: var(--dark-3); overflow: hidden; transition: background 0.3s; }
  .garantie-item:hover { background: var(--dark-4); }
  .garantie-icon { background: rgba(201,168,76,0.1); display: flex; align-items: center; justify-content: center; font-size: 20px; }
  .garantie-content { padding: 20px 24px; }
  .garantie-name { font-size: 13px; font-weight: 500; color: var(--white); margin-bottom: 4px; }
  .garantie-detail { font-size: 11px; color: var(--white-dim); letter-spacing: 0.5px; }
  .garanties-right { display: flex; flex-direction: column; gap: 2px; }
  .engage-item { padding: 28px 32px; background: var(--dark-3); display: flex; align-items: flex-start; gap: 20px; border-left: 2px solid transparent; transition: all 0.3s; }
  .engage-item:hover { border-left-color: var(--gold); background: var(--dark-4); }
  .engage-check { color: var(--gold); font-size: 18px; flex-shrink: 0; margin-top: 2px; }
  .engage-text { font-size: 14px; color: var(--white-dim); line-height: 1.5; }
  .engage-text strong { color: var(--white); display: block; margin-bottom: 4px; font-size: 15px; }

  .zones { padding: 100px 80px; background: var(--dark); text-align: center; }
  .zones .section-label { justify-content: center; }
  .zones .section-label::before { display: none; }
  .zones .section-title { text-align: center; margin-bottom: 60px; }
  .zones-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 2px; max-width: 800px; margin: 0 auto 60px; }
  .zone-card { background: var(--dark-3); padding: 40px 32px; border-top: 2px solid transparent; transition: all 0.3s; }
  .zone-card:hover { border-top-color: var(--gold); background: var(--dark-4); }
  .zone-card.main { border-top-color: var(--gold); }
  .zone-city { font-family: 'Bebas Neue', sans-serif; font-size: 28px; letter-spacing: 2px; margin-bottom: 8px; }
  .zone-region { font-size: 11px; letter-spacing: 2px; text-transform: uppercase; color: var(--gold); }
  .zone-detail { margin-top: 12px; font-size: 12px; color: var(--white-dim); }

  .contact { padding: 120px 80px; background: var(--dark-3); display: grid; grid-template-columns: 1fr 1fr; gap: 80px; }
  .contact-info { display: flex; flex-direction: column; gap: 40px; }
  .contact-block { display: flex; flex-direction: column; gap: 6px; }
  .contact-block-label { font-size: 10px; letter-spacing: 3px; text-transform: uppercase; color: var(--gold); }
  .contact-block-val { font-family: 'Bebas Neue', sans-serif; font-size: 28px; letter-spacing: 2px; color: var(--white); }
  .contact-block-sub { font-size: 12px; color: var(--white-dim); }
  .contact-emails { display: flex; flex-direction: column; gap: 6px; }
  .contact-email { font-size: 13px; color: var(--white-dim); transition: color 0.3s; }
  .contact-email:hover { color: var(--gold); }
  .contact-form-side { display: flex; flex-direction: column; gap: 2px; }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 2px; }
  .form-field { background: var(--dark-4); display: flex; flex-direction: column; }
  .form-field label { padding: 12px 20px 0; font-size: 9px; letter-spacing: 2px; text-transform: uppercase; color: var(--gold); }
  .form-field input, .form-field select, .form-field textarea { background: transparent; border: none; outline: none; padding: 8px 20px 16px; color: var(--white); font-family: 'DM Sans', sans-serif; font-size: 14px; resize: none; }
  .form-field select option { background: var(--dark); }
  .form-submit { background: var(--gold); border: none; color: var(--dark); padding: 20px; font-family: 'Bebas Neue', sans-serif; font-size: 18px; letter-spacing: 3px; cursor: pointer; transition: background 0.3s; margin-top: 2px; }
  .form-submit:hover { background: var(--gold-light); }

  footer { background: var(--dark); padding: 48px 80px; display: grid; grid-template-columns: 1fr auto 1fr; align-items: center; border-top: 1px solid rgba(201,168,76,0.15); }
  .footer-logo { font-family: 'Bebas Neue', sans-serif; font-size: 32px; letter-spacing: 4px; color: var(--white); }
  .footer-logo span { color: var(--gold); }
  .footer-siren { text-align: center; font-size: 11px; color: var(--white-dim); letter-spacing: 1px; }
  .footer-siren strong { color: var(--gold); display: block; margin-bottom: 4px; }
  .footer-right { text-align: right; font-size: 11px; color: var(--white-dim); }

  @keyframes fadeUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes float { 0%, 100% { transform: translateY(0) scale(1); } 50% { transform: translateY(-20px) scale(1.02); } }
  @keyframes scroll { from { transform: translateX(0); } to { transform: translateX(-50%); } }
  .reveal { opacity: 0; transform: translateY(40px); transition: opacity 0.8s ease, transform 0.8s ease; }
  .reveal.visible { opacity: 1; transform: none; }
  .hamburger { display: none; }

  @media (max-width: 1024px) {
    nav { padding: 20px 24px; }
    .nav-links { display: none; }
    .hamburger { display: block; background: none; border: 1px solid var(--gold); color: var(--gold); padding: 8px 12px; font-size: 11px; letter-spacing: 2px; cursor: pointer; }
    .hero { grid-template-columns: 1fr; min-height: auto; }
    .hero-left { padding: 100px 24px 60px; }
    .hero-right { display: none; }
    .services { padding: 80px 24px; }
    .services-grid { grid-template-columns: repeat(2, 1fr); }
    .section-header { grid-template-columns: 1fr; gap: 24px; }
    .tarifs { padding: 80px 24px; }
    .tarifs-intro { grid-template-columns: 1fr; gap: 40px; }
    .garanties { grid-template-columns: 1fr; padding: 80px 24px; gap: 40px; }
    .zones { padding: 80px 24px; }
    .zones-grid { grid-template-columns: 1fr; }
    .contact { grid-template-columns: 1fr; padding: 80px 24px; gap: 40px; }
    .form-row { grid-template-columns: 1fr; }
    footer { grid-template-columns: 1fr; gap: 20px; padding: 40px 24px; text-align: center; }
    .footer-right { text-align: center; }
    .tarif-row { grid-template-columns: 70px 1fr 100px; }
    .tarif-unit { display: none; }
    body { cursor: auto; }
    .cursor, .cursor-ring { display: none; }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>

<nav>
  <a href="#" class="nav-logo">NNB<span>·</span>POLYBAT</a>
  <ul class="nav-links">
    <li><a href="#services">Services</a></li>
    <li><a href="#tarifs">Tarifs</a></li>
    <li><a href="#garanties">Garanties</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="tel:0490212244" class="nav-cta">Appeler</a></li>
  </ul>
  <button class="hamburger" onclick="alert('04.90.21.22.44')">MENU</button>
</nav>

<section class="hero">
  <div class="hero-bg"></div>
  <div class="hero-grid"></div>
  <div class="hero-left">
    <div class="hero-badge"><span>Tous Corps d'État — Depuis 2013</span></div>
    <h1 class="hero-title">NNB<br><span class="gold">POLY</span>BAT</h1>
    <p class="hero-subtitle">L'excellence technique au service<br>de votre patrimoine</p>
    <div class="hero-stats">
      <div class="stat"><div class="stat-num">10+</div><div class="stat-label">Ans d'expertise</div></div>
      <div class="stat"><div class="stat-num">−25%</div><div class="stat-label">Vs marché</div></div>
      <div class="stat"><div class="stat-num">2–4h</div><div class="stat-label">Urgences</div></div>
    </div>
    <div class="hero-phones">
      <div class="phone-item"><span class="phone-label">Accueil</span><a href="tel:0490212244" class="phone-num" style="color:inherit;text-decoration:none;">04.90.21.22.44</a></div>
      <div class="phone-item"><span class="phone-label">Technique</span><a href="tel:0632248314" class="phone-num" style="color:inherit;text-decoration:none;">06.32.24.83.14</a></div>
    </div>
  </div>
  <div class="hero-right">
    <div class="hero-divider"></div>
    <div class="hero-visual">
      <div class="hero-monogram">NNB</div>
      <div class="hero-overlay-text">
        <div class="certif-badge"><span class="certif-title">Garantie</span><span class="certif-val">Décennale</span></div>
        <div class="certif-badge"><span class="certif-title">RC Pro</span><span class="certif-val">Couverture illimitée</span></div>
        <div class="certif-badge"><span class="certif-title">SIREN</span><span class="certif-val">798 195 657</span></div>
      </div>
    </div>
  </div>
</section>

<div class="banner">
  <div class="banner-track">
    <div class="banner-item"><span>✓ Garantie Décennale</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Tarifs −25% vs marché</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Urgences 7j/7</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Milwaukee · Hilti · Festool</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Devis sous 24–48h</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Garantie Décennale</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Tarifs −25% vs marché</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Urgences 7j/7</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Milwaukee · Hilti · Festool</span></div><div class="banner-dot"></div>
    <div class="banner-item"><span>✓ Devis sous 24–48h</span></div><div class="banner-dot"></div>
  </div>
</div>

<section class="services" id="services">
  <div class="section-header reveal">
    <div><div class="section-label">Nos expertises</div><h2 class="section-title">10 CORPS<br>DE MÉTIER</h2></div>
    <p class="section-desc">Une équipe pluridisciplinaire pour tous vos besoins de rénovation, maintenance et gestion immobilière en Occitanie et Île-de-France.</p>
  </div>
  <div class="services-grid">
    <div class="service-card reveal"><div class="service-num">01</div><div class="service-icon">🔧</div><div class="service-name">Plomberie & Sanitaires</div><ul class="service-items"><li>Robinetterie</li><li>Sanitaires & WC</li><li>Chauffe-eau</li><li>Canalisations</li></ul></div>
    <div class="service-card reveal"><div class="service-num">02</div><div class="service-icon">⚡</div><div class="service-name">Électricité & VMC</div><ul class="service-items"><li>Tableaux électriques</li><li>Appareillage & prises</li><li>Éclairage LED</li><li>Chauffage & VMC</li></ul></div>
    <div class="service-card reveal"><div class="service-num">03</div><div class="service-icon">🖌️</div><div class="service-name">Peinture & Revêtements</div><ul class="service-items"><li>Préparation supports</li><li>Peinture pro</li><li>Boiseries</li><li>Revêtements muraux</li></ul></div>
    <div class="service-card reveal"><div class="service-num">04</div><div class="service-icon">🏗️</div><div class="service-name">Placo & Isolation</div><ul class="service-items"><li>Cloisons & doublages</li><li>Faux-plafonds</li><li>Isolation thermique</li><li>Finitions</li></ul></div>
    <div class="service-card reveal"><div class="service-num">05</div><div class="service-icon">🚪</div><div class="service-name">Menuiserie Intérieure</div><ul class="service-items"><li>Portes & huisseries</li><li>Placards</li><li>Plans de travail</li><li>Plinthes</li></ul></div>
    <div class="service-card reveal"><div class="service-num">06</div><div class="service-icon">🔐</div><div class="service-name">Serrurerie & Sécurité</div><ul class="service-items"><li>Dépannage urgence</li><li>Serrures A2P</li><li>Fenêtres & volets</li><li>Contrôle d'accès</li></ul></div>
    <div class="service-card reveal"><div class="service-num">07</div><div class="service-icon">✨</div><div class="service-name">Nettoyage Professionnel</div><ul class="service-items"><li>Fin de bail</li><li>Après travaux</li><li>Désinfection</li><li>Prestations spécialisées</li></ul></div>
    <div class="service-card reveal"><div class="service-num">08</div><div class="service-icon">🏠</div><div class="service-name">Carrelage & Sols</div><ul class="service-items"><li>Carrelage</li><li>Parquet</li><li>Sols stratifiés</li><li>Sols souples</li></ul></div>
    <div class="service-card reveal"><div class="service-num">09</div><div class="service-icon">🏛️</div><div class="service-name">Maçonnerie & Gros Œuvre</div><ul class="service-items"><li>Maçonnerie</li><li>Chapes & dalles</li><li>Façades</li><li>Étanchéité</li></ul></div>
    <div class="service-card reveal"><div class="service-num">10</div><div class="service-icon">📋</div><div class="service-name">Divers & Prestations</div><ul class="service-items"><li>Main d'œuvre</li><li>Diagnostics</li><li>Packs locatifs</li><li>Administratif</li></ul></div>
  </div>
</section>

<section class="tarifs" id="tarifs">
  <div class="tarifs-intro reveal">
    <div><div class="section-label">BPU 2026</div><h2 class="section-title">TARIFS<br>TRANSPARENTS</h2></div>
    <p class="section-desc">Facturation au réel avec références BPU. Pas de frais cachés. Remise automatique −10% sur tout devis &gt; 500€ HT.</p>
  </div>
  <div class="tarif-tabs reveal">
    <button class="tab-btn active" onclick="showTab('plomberie',this)">Plomberie</button>
    <button class="tab-btn" onclick="showTab('electricite',this)">Électricité</button>
    <button class="tab-btn" onclick="showTab('peinture',this)">Peinture</button>
    <button class="tab-btn" onclick="showTab('carrelage',this)">Carrelage</button>
    <button class="tab-btn" onclick="showTab('nettoyage',this)">Nettoyage</button>
    <button class="tab-btn" onclick="showTab('serrurerie',this)">Serrurerie</button>
    <button class="tab-btn" onclick="showTab('divers',this)">Divers</button>
  </div>
  <div id="plomberie" class="tarif-table active reveal">
    <div class="tarif-cat-title">1.1 — Robinetterie</div>
    <div class="tarif-row"><div class="tarif-code">PL01</div><div class="tarif-desc">Remplacement mitigeur cuisine</div><div class="tarif-unit">U</div><div class="tarif-price">145 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL02</div><div class="tarif-desc">Remplacement mitigeur lavabo</div><div class="tarif-unit">U</div><div class="tarif-price">145 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL03</div><div class="tarif-desc">Remplacement mitigeur douche encastré</div><div class="tarif-unit">U</div><div class="tarif-price">185 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL05</div><div class="tarif-desc">Remplacement tête de robinet céramique</div><div class="tarif-unit">U</div><div class="tarif-price">45 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL09</div><div class="tarif-desc">Remplacement cartouche thermostatique</div><div class="tarif-unit">U</div><div class="tarif-price">95 €</div></div>
    <div class="tarif-cat-title">1.2 — Sanitaires & WC</div>
    <div class="tarif-row"><div class="tarif-code">PL11</div><div class="tarif-desc">Remplacement mécanisme chasse d'eau simple</div><div class="tarif-unit">U</div><div class="tarif-price">95 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL13</div><div class="tarif-desc">Pose WC standard complet</div><div class="tarif-unit">U</div><div class="tarif-price">390 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL14</div><div class="tarif-desc">Pose WC suspendu complet</div><div class="tarif-unit">U</div><div class="tarif-price">680 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL17</div><div class="tarif-desc">Pose receveur de douche 80×80 cm</div><div class="tarif-unit">U</div><div class="tarif-price">425 €</div></div>
    <div class="tarif-cat-title">1.3 — Chauffe-eau</div>
    <div class="tarif-row"><div class="tarif-code">PL21</div><div class="tarif-desc">Chauffe-eau 50 L vertical mural</div><div class="tarif-unit">U</div><div class="tarif-price">440 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL22</div><div class="tarif-desc">Chauffe-eau 100 L vertical mural</div><div class="tarif-unit">U</div><div class="tarif-price">515 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL23</div><div class="tarif-desc">Chauffe-eau 150 L vertical mural</div><div class="tarif-unit">U</div><div class="tarif-price">595 €</div></div>
    <div class="tarif-cat-title">1.4 — Canalisations</div>
    <div class="tarif-row"><div class="tarif-code">PL31</div><div class="tarif-desc">Débouchage urgence (évier, lavabo, WC)</div><div class="tarif-unit">U</div><div class="tarif-price">95 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PL33</div><div class="tarif-desc">Recherche de fuite avec rapport assurance</div><div class="tarif-unit">U</div><div class="tarif-price">155 €</div></div>
  </div>
  <div id="electricite" class="tarif-table reveal">
    <div class="tarif-cat-title">2.1 — Tableaux électriques</div>
    <div class="tarif-row"><div class="tarif-code">EL01</div><div class="tarif-desc">Tableau électrique complet NF C 15-100</div><div class="tarif-unit">U</div><div class="tarif-price">790 €</div></div>
    <div class="tarif-row"><div class="tarif-code">EL03</div><div class="tarif-desc">Disjoncteur différentiel 30mA type A</div><div class="tarif-unit">U</div><div class="tarif-price">120 €</div></div>
    <div class="tarif-row"><div class="tarif-code">EL09</div><div class="tarif-desc">Diagnostic électrique complet avec rapport</div><div class="tarif-unit">U</div><div class="tarif-price">245 €</div></div>
    <div class="tarif-cat-title">2.2 — Appareillage</div>
    <div class="tarif-row"><div class="tarif-code">EL11</div><div class="tarif-desc">Prise électrique 16A gamme professionnelle</div><div class="tarif-unit">U</div><div class="tarif-price">28 €</div></div>
    <div class="tarif-row"><div class="tarif-code">EL14</div><div class="tarif-desc">Prise électrique encastrée avec saignée</div><div class="tarif-unit">U</div><div class="tarif-price">85 €</div></div>
    <div class="tarif-cat-title">2.3 — Éclairage</div>
    <div class="tarif-row"><div class="tarif-code">EL21</div><div class="tarif-desc">Spot LED encastrable avec perçage</div><div class="tarif-unit">U</div><div class="tarif-price">48 €</div></div>
    <div class="tarif-row"><div class="tarif-code">EL27</div><div class="tarif-desc">Détecteur de fumée DAAF normé</div><div class="tarif-unit">U</div><div class="tarif-price">45 €</div></div>
    <div class="tarif-cat-title">2.4 — VMC & Chauffage</div>
    <div class="tarif-row"><div class="tarif-code">EL35</div><div class="tarif-desc">Remplacement moteur VMC simple flux</div><div class="tarif-unit">U</div><div class="tarif-price">240 €</div></div>
    <div class="tarif-row"><div class="tarif-code">EL39</div><div class="tarif-desc">VMC double flux avec récupérateur</div><div class="tarif-unit">U</div><div class="tarif-price">1 850 €</div></div>
  </div>
  <div id="peinture" class="tarif-table reveal">
    <div class="tarif-cat-title">3.1 — Préparation</div>
    <div class="tarif-row"><div class="tarif-code">PE01</div><div class="tarif-desc">Lessivage murs et plafonds</div><div class="tarif-unit">M²</div><div class="tarif-price">3,80 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PE05</div><div class="tarif-desc">Enduit de lissage complet au ratissage</div><div class="tarif-unit">M²</div><div class="tarif-price">13 €</div></div>
    <div class="tarif-cat-title">3.2 — Peinture</div>
    <div class="tarif-row"><div class="tarif-code">PE11</div><div class="tarif-desc">Peinture mate plafonds qualité pro – 2 couches</div><div class="tarif-unit">M²</div><div class="tarif-price">15 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PE12</div><div class="tarif-desc">Peinture satinée murs lessivable A+ – 2 couches</div><div class="tarif-unit">M²</div><div class="tarif-price">17,50 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PE15</div><div class="tarif-desc">Peinture antimoisissure cuisine/SDB</div><div class="tarif-unit">M²</div><div class="tarif-price">18,50 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PE16</div><div class="tarif-desc">Peinture effet béton ciré (3 couches + protection)</div><div class="tarif-unit">M²</div><div class="tarif-price">45 €</div></div>
    <div class="tarif-cat-title">3.4 — Revêtements</div>
    <div class="tarif-row"><div class="tarif-code">PE31</div><div class="tarif-desc">Pose papier peint intissé standard</div><div class="tarif-unit">M²</div><div class="tarif-price">18 €</div></div>
    <div class="tarif-row"><div class="tarif-code">PE37</div><div class="tarif-desc">Pose carrelage mural faïence</div><div class="tarif-unit">M²</div><div class="tarif-price">45 €</div></div>
  </div>
  <div id="carrelage" class="tarif-table reveal">
    <div class="tarif-cat-title">8.1 — Carrelage sols</div>
    <div class="tarif-row"><div class="tarif-code">CA01</div><div class="tarif-desc">Carrelage sol 30×30 à 45×45 cm scellé</div><div class="tarif-unit">M²</div><div class="tarif-price">45 €</div></div>
    <div class="tarif-row"><div class="tarif-code">CA02</div><div class="tarif-desc">Carrelage grand format 60×60 à 80×80 cm</div><div class="tarif-unit">M²</div><div class="tarif-price">55 €</div></div>
    <div class="tarif-row"><div class="tarif-code">CA04</div><div class="tarif-desc">Carrelage extérieur antidérapant terrasse</div><div class="tarif-unit">M²</div><div class="tarif-price">52 €</div></div>
    <div class="tarif-cat-title">8.3 — Parquets</div>
    <div class="tarif-row"><div class="tarif-code">CA21</div><div class="tarif-desc">Parquet flottant stratifié clipsable</div><div class="tarif-unit">M²</div><div class="tarif-price">28 €</div></div>
    <div class="tarif-row"><div class="tarif-code">CA25</div><div class="tarif-desc">Ponçage parquet massif 3 passes avec finition</div><div class="tarif-unit">M²</div><div class="tarif-price">35 €</div></div>
    <div class="tarif-row"><div class="tarif-code">CA30</div><div class="tarif-desc">Rénovation parquet ancien (ponçage + vitrif)</div><div class="tarif-unit">M²</div><div class="tarif-price">48 €</div></div>
  </div>
  <div id="nettoyage" class="tarif-table reveal">
    <div class="tarif-cat-title">7.1 — Fin de bail</div>
    <div class="tarif-row"><div class="tarif-code">NT01</div><div class="tarif-desc">Nettoyage studio &lt; 20m²</div><div class="tarif-unit">U</div><div class="tarif-price">185 €</div></div>
    <div class="tarif-row"><div class="tarif-code">NT02</div><div class="tarif-desc">Nettoyage T1 20-30m²</div><div class="tarif-unit">U</div><div class="tarif-price">225 €</div></div>
    <div class="tarif-row"><div class="tarif-code">NT03</div><div class="tarif-desc">Nettoyage T2 30-45m²</div><div class="tarif-unit">U</div><div class="tarif-price">270 €</div></div>
    <div class="tarif-row"><div class="tarif-code">NT04</div><div class="tarif-desc">Nettoyage T3 45-65m²</div><div class="tarif-unit">U</div><div class="tarif-price">345 €</div></div>
    <div class="tarif-row"><div class="tarif-code">NT05</div><div class="tarif-desc">Nettoyage T4 65-85m²</div><div class="tarif-unit">U</div><div class="tarif-price">425 €</div></div>
    <div class="tarif-cat-title">7.2 — Par zones</div>
    <div class="tarif-row"><div class="tarif-code">NT11</div><div class="tarif-desc">Nettoyage cuisine complète (four, plaques, hotte)</div><div class="tarif-unit">U</div><div class="tarif-price">95 €</div></div>
    <div class="tarif-row"><div class="tarif-code">NT12</div><div class="tarif-desc">Nettoyage salle de bain complète</div><div class="tarif-unit">U</div><div class="tarif-price">85 €</div></div>
  </div>
  <div id="serrurerie" class="tarif-table reveal">
    <div class="tarif-cat-title">6.1 — Dépannage urgence</div>
    <div class="tarif-row"><div class="tarif-code">SE01</div><div class="tarif-desc">Ouverture porte claquée sans dégâts</div><div class="tarif-unit">U</div><div class="tarif-price">85 €</div></div>
    <div class="tarif-row"><div class="tarif-code">SE02</div><div class="tarif-desc">Ouverture porte fermée à clé</div><div class="tarif-unit">U</div><div class="tarif-price">125 €</div></div>
    <div class="tarif-row"><div class="tarif-code">SE10</div><div class="tarif-desc">Supplément intervention urgence 24h/24</div><div class="tarif-unit">U</div><div class="tarif-price">65 €</div></div>
    <div class="tarif-cat-title">6.2 — Serrures</div>
    <div class="tarif-row"><div class="tarif-code">SE11</div><div class="tarif-desc">Cylindre européen standard (3 clés)</div><div class="tarif-unit">U</div><div class="tarif-price">95 €</div></div>
    <div class="tarif-row"><div class="tarif-code">SE12</div><div class="tarif-desc">Cylindre haute sécurité A2P*</div><div class="tarif-unit">U</div><div class="tarif-price">165 €</div></div>
    <div class="tarif-row"><div class="tarif-code">SE16</div><div class="tarif-desc">Serrure 5 points encastrée haute sécurité</div><div class="tarif-unit">U</div><div class="tarif-price">685 €</div></div>
  </div>
  <div id="divers" class="tarif-table reveal">
    <div class="tarif-cat-title">10.1 — Main d'œuvre</div>
    <div class="tarif-row"><div class="tarif-code">DI01</div><div class="tarif-desc">Main d'œuvre expert (plomberie, électricité)</div><div class="tarif-unit">H</div><div class="tarif-price">48 €</div></div>
    <div class="tarif-row"><div class="tarif-code">DI02</div><div class="tarif-desc">Main d'œuvre polyvalente (peinture, menuiserie)</div><div class="tarif-unit">H</div><div class="tarif-price">40 €</div></div>
    <div class="tarif-row"><div class="tarif-code">DI10</div><div class="tarif-desc">Majoration week-end/jours fériés</div><div class="tarif-unit">%</div><div class="tarif-price">+35%</div></div>
    <div class="tarif-cat-title">10.3 — Packs locatifs</div>
    <div class="tarif-row"><div class="tarif-code">DI22</div><div class="tarif-desc">Pack Rénovation Peinture Studio &lt; 20m²</div><div class="tarif-unit">U</div><div class="tarif-price gold">1 250 €</div></div>
    <div class="tarif-row"><div class="tarif-code">DI23</div><div class="tarif-desc">Pack Rénovation Peinture T1 (20-30m²)</div><div class="tarif-unit">U</div><div class="tarif-price gold">1 750 €</div></div>
    <div class="tarif-row"><div class="tarif-code">DI24</div><div class="tarif-desc">Pack Rénovation Peinture T2 (30-45m²)</div><div class="tarif-unit">U</div><div class="tarif-price gold">2 450 €</div></div>
    <div class="tarif-cat-title">10.4 — Services administratifs</div>
    <div class="tarif-row"><div class="tarif-code">DI15</div><div class="tarif-desc">Pré-visite technique (déduite si travaux)</div><div class="tarif-unit">U</div><div class="tarif-price gold">Gratuit</div></div>
    <div class="tarif-row"><div class="tarif-code">DI37</div><div class="tarif-desc">Devis détaillé sous 24-48h</div><div class="tarif-unit">U</div><div class="tarif-price gold">Gratuit</div></div>
  </div>
</section>

<section class="garanties" id="garanties">
  <div class="garanties-left reveal">
    <div class="section-label">Nos garanties</div>
    <h2 class="section-title">SÉCURITÉ<br>TOTALE</h2>
    <div class="garantie-items">
      <div class="garantie-item"><div class="garantie-icon">🏛️</div><div class="garantie-content"><div class="garantie-name">Garantie Décennale</div><div class="garantie-detail">10 ans sur les éléments de structure</div></div></div>
      <div class="garantie-item"><div class="garantie-icon">⚙️</div><div class="garantie-content"><div class="garantie-name">Garantie Biennale</div><div class="garantie-detail">2 ans sur les équipements dissociables</div></div></div>
      <div class="garantie-item"><div class="garantie-icon">✅</div><div class="garantie-content"><div class="garantie-name">Parfait Achèvement</div><div class="garantie-detail">1 an sur toutes les prestations</div></div></div>
      <div class="garantie-item"><div class="garantie-icon">🛡️</div><div class="garantie-content"><div class="garantie-name">RC Professionnelle</div><div class="garantie-detail">Couverture illimitée — Attestations sur demande</div></div></div>
    </div>
  </div>
  <div class="garanties-right reveal">
    <div class="engage-item"><div class="engage-check">✦</div><div class="engage-text"><strong>Tarification 25% sous la moyenne du marché</strong>Remise automatique −10% sur tout devis supérieur à 500€ HT</div></div>
    <div class="engage-item"><div class="engage-check">✦</div><div class="engage-text"><strong>Urgences 7j/7 — Intervention sous 2–4h</strong>Sur Montpellier Métropole. Suppléments transparents</div></div>
    <div class="engage-item"><div class="engage-check">✦</div><div class="engage-text"><strong>Outillage 100% électrique professionnel</strong>Milwaukee · Hilti · Festool. Zéro nuisances inutiles</div></div>
    <div class="engage-item"><div class="engage-check">✦</div><div class="engage-text"><strong>Devis détaillé sous 24–48h</strong>Pré-visite gratuite, déduite en cas de travaux</div></div>
    <div class="engage-item"><div class="engage-check">✦</div><div class="engage-text"><strong>Photos HD avant/après horodatées</strong>Rapport complet de fin de chantier inclus</div></div>
    <div class="engage-item"><div class="engage-check">✦</div><div class="engage-text"><strong>Facturation au réel avec codes BPU</strong>Zéro frais cachés. Transparence totale</div></div>
  </div>
</section>

<section class="zones" id="zones">
  <div class="section-label reveal">Zones d'intervention</div>
  <h2 class="section-title reveal">MONTPELLIER<br>& ÎLE-DE-FRANCE</h2>
  <div class="zones-grid reveal">
    <div class="zone-card main"><div class="zone-city">Montpellier</div><div class="zone-region">Occitanie — Zone 1</div><div class="zone-detail">Urgences sous 2h<br>Métropole & alentours</div></div>
    <div class="zone-card"><div class="zone-city">Occitanie</div><div class="zone-region">Zone 2 — 30 à 150km</div><div class="zone-detail">Déplacement 40–120€<br>Tout le département</div></div>
    <div class="zone-card"><div class="zone-city">Paris</div><div class="zone-region">Île-de-France — National</div><div class="zone-detail">0,60€/km<br>Déplacements nationaux</div></div>
  </div>
  <div style="margin-top:20px;font-size:12px;color:var(--white-dim);letter-spacing:1px;">Disponible Lundi–Vendredi 8h–18h &nbsp;|&nbsp; Urgences 7j/7</div>
</section>

<section class="contact" id="contact">
  <div class="contact-info reveal">
    <div><div class="section-label">Nous contacter</div><h2 class="section-title">DEMANDE<br>DE DEVIS</h2></div>
    <div class="contact-block"><div class="contact-block-label">Accueil & Gestion</div><a href="tel:0490212244" class="contact-block-val" style="text-decoration:none;color:inherit;">04.90.21.22.44</a><div class="contact-block-sub">Lundi–Vendredi 8h–18h</div></div>
    <div class="contact-block"><div class="contact-block-label">Service Technique</div><a href="tel:0632248314" class="contact-block-val" style="text-decoration:none;color:inherit;">06.32.24.83.14</a><div class="contact-block-sub">Urgences 7j/7</div></div>
    <div class="contact-block"><div class="contact-block-label">Email</div><div class="contact-emails"><a href="mailto:nnb.polybat@gmail.com" class="contact-email">nnb.polybat@gmail.com</a><a href="mailto:nnb.polybat-occitanie@gmail.com" class="contact-email">nnb.polybat-occitanie@gmail.com</a><a href="mailto:nnb.polybat-iledefrance@gmail.com" class="contact-email">nnb.polybat-iledefrance@gmail.com</a></div></div>
  </div>
  <div class="contact-form-side reveal">
    <div class="form-row">
      <div class="form-field"><label>Nom</label><input type="text" placeholder="Votre nom"></div>
      <div class="form-field"><label>Téléphone</label><input type="tel" placeholder="06 XX XX XX XX"></div>
    </div>
    <div class="form-field"><label>Email</label><input type="email" placeholder="votre@email.com"></div>
    <div class="form-field"><label>Type de prestation</label><select><option>Plomberie & Sanitaires</option><option>Électricité & VMC</option><option>Peinture & Revêtements</option><option>Carrelage & Sols</option><option>Serrurerie & Sécurité</option><option>Nettoyage professionnel</option><option>Maçonnerie & Gros Œuvre</option><option>Menuiserie intérieure</option><option>Pack gestion locative</option><option>Autre</option></select></div>
    <div class="form-field"><label>Description des travaux</label><textarea rows="4" placeholder="Décrivez votre besoin, surface, contexte..."></textarea></div>
    <button class="form-submit" onclick="alert('Merci ! Nous vous contacterons sous 24h.\nTél : 04.90.21.22.44')">DEMANDER UN DEVIS GRATUIT →</button>
  </div>
</section>

<footer>
  <div class="footer-logo">NNB<span>·</span>POLYBAT</div>
  <div class="footer-siren"><strong>SIREN 798 195 657</strong>Garantie Décennale · RC Professionnelle · Depuis 2013</div>
  <div class="footer-right">Montpellier · Occitanie<br>Paris · Île-de-France<br><span style="color:var(--gold);">L'excellence technique au service de votre patrimoine</span></div>
</footer>

<script>
  const cursor = document.getElementById('cursor');
  const ring = document.getElementById('cursorRing');
  let mx=0,my=0,rx=0,ry=0;
  document.addEventListener('mousemove',e=>{mx=e.clientX;my=e.clientY;cursor.style.left=mx+'px';cursor.style.top=my+'px';});
  function animRing(){rx+=(mx-rx)*0.12;ry+=(my-ry)*0.12;ring.style.left=rx+'px';ring.style.top=ry+'px';requestAnimationFrame(animRing);}
  animRing();
  const observer=new IntersectionObserver(entries=>{entries.forEach((e,i)=>{if(e.isIntersecting)setTimeout(()=>e.target.classList.add('visible'),i*80);});},{threshold:0.1});
  document.querySelectorAll('.reveal').forEach(el=>observer.observe(el));
  function showTab(id,btn){document.querySelectorAll('.tarif-table').forEach(t=>t.classList.remove('active'));document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));document.getElementById(id).classList.add('active');btn.classList.add('active');}
</script>
</body>
</html>
