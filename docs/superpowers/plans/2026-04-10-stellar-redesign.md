# Stellar.ai Redesign — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remplacer le design Track Night (fond sombre) par le design Stellar.ai (fond blanc, Inter, animations fadeInUp, tabs interactifs) avec contenu FrankenPHP/Symfony/Clever Cloud.

**Architecture:** Réécriture de deux fichiers Twig uniquement — `base.html.twig` pour la fondation CSS/animations, `templates/homepage/index.html.twig` pour tout le contenu HTML, les styles page-specific et le JS vanilla. Aucune dépendance externe ajoutée, aucun build step.

**Tech Stack:** Twig, CSS natif, vanilla JS, Inter via Google Fonts, vidéo CloudFront en fond.

---

## Fichiers modifiés

| Fichier | Action | Responsabilité |
|---------|--------|----------------|
| `templates/base.html.twig` | Modifier | Font Inter, reset CSS blanc, keyframes animations |
| `templates/homepage/index.html.twig` | Réécrire | Tout le HTML (nav, hero, tabs, overlays, logos, footer) + CSS page + JS tabs |

---

## Task 1 : Réécriture de base.html.twig (fondation CSS)

**Files:**
- Modify: `templates/base.html.twig`

- [ ] **Step 1 : Remplacer le contenu complet de base.html.twig**

```twig
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}FrankenPHP on Clever Cloud{% endblock %}</title>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

    <style>
      * { box-sizing: border-box; margin: 0; padding: 0; }

      html, body {
        min-height: 100%;
        font-family: 'Inter', sans-serif;
        background: #ffffff;
        color: #111827;
      }

      a { color: inherit; text-decoration: none; }

      .page-wrapper {
        max-width: 1280px;
        margin: 0 auto;
        padding: 0 24px;
      }

      @keyframes fadeInUp {
        from { opacity: 0; transform: translateY(30px); }
        to   { opacity: 1; transform: translateY(0); }
      }
      .animate-fade-in-up {
        animation: fadeInUp 0.6s ease-out forwards;
      }

      @keyframes fadeInOverlay {
        from { opacity: 0; }
        to   { opacity: 1; }
      }
      .animate-fade-in-overlay {
        animation: fadeInOverlay 0.4s ease-out forwards;
      }

      @keyframes fadeInDialog {
        from { opacity: 0; }
        to   { opacity: 1; }
      }
      .animate-slide-up-overlay {
        animation: fadeInDialog 0.5s ease-out forwards;
      }
    </style>

    {% block stylesheets %}{% endblock %}
    {% block javascripts %}{% endblock %}
  </head>
  <body>
    {% block body %}{% endblock %}
  </body>
</html>
```

- [ ] **Step 2 : Vérifier la syntaxe Twig**

```bash
cd /Users/vitio/ClaudeByVitio/demo-php-frankenphp
# Vérifier qu'aucune balise Twig n'est cassée
grep -n "{%" templates/base.html.twig
```

Résultat attendu : 3 lignes `{% block ... %}{% endblock %}` visibles.

- [ ] **Step 3 : Commit**

```bash
git add templates/base.html.twig
git commit -m "style: rewrite base template with Stellar.ai foundation (Inter, white bg, animations)"
```

---

## Task 2 : Navigation dans index.html.twig

**Files:**
- Modify: `templates/homepage/index.html.twig`

- [ ] **Step 1 : Remplacer index.html.twig avec la nav et son CSS**

```twig
{% extends 'base.html.twig' %}

{% block title %}FrankenPHP + Symfony on Clever Cloud{% endblock %}

{% block body %}

<style>
  /* ── NAV ── */
  .nav {
    position: sticky;
    top: 0;
    z-index: 50;
    background: #ffffff;
    border-bottom: 1px solid #f0f0f0;
  }
  .nav-inner {
    max-width: 1280px;
    margin: 0 auto;
    padding: 16px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .nav-left {
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .nav-brand {
    font-size: 18px;
    font-weight: 600;
    color: #111827;
  }
  .nav-center {
    display: none;
    align-items: center;
    gap: 32px;
  }
  @media (min-width: 768px) {
    .nav-center { display: flex; }
  }
  .nav-center a {
    display: flex;
    align-items: center;
    gap: 4px;
    font-size: 14px;
    color: #374151;
    transition: color 0.15s;
  }
  .nav-center a:hover { color: #111827; }
  .nav-right {
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .nav-right .nav-link {
    font-size: 14px;
    color: #374151;
  }
  .nav-right .nav-link:hover { color: #111827; }
  .btn-primary {
    display: inline-flex;
    align-items: center;
    background: #111827;
    color: #ffffff !important;
    padding: 10px 20px;
    border-radius: 9999px;
    font-size: 14px;
    font-weight: 500;
    transition: background 0.15s;
    white-space: nowrap;
  }
  .btn-primary:hover { background: #374151; }
</style>

<!-- NAVIGATION -->
<nav class="nav animate-fade-in-up" style="opacity:0;animation-delay:0.1s">
  <div class="nav-inner">
    <div class="nav-left">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="#111827"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>
      <span class="nav-brand">FrankenPHP</span>
    </div>
    <div class="nav-center">
      <a href="https://frankenphp.dev/docs/" target="_blank" rel="noopener">
        Runtimes
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
      </a>
      <a href="https://www.clever-cloud.com/product/add-ons/" target="_blank" rel="noopener">
        Add-ons
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
      </a>
      <a href="https://developers.clever-cloud.com/doc/applications/docker/" target="_blank" rel="noopener">Deploy</a>
      <a href="https://developers.clever-cloud.com/doc/" target="_blank" rel="noopener">Docs</a>
    </div>
    <div class="nav-right">
      <a class="nav-link" href="https://www.clever-cloud.com" target="_blank" rel="noopener">clever-cloud.com</a>
      <a class="btn-primary" href="/api" target="_blank" rel="noopener">Open Swagger UI</a>
    </div>
  </div>
</nav>

{% endblock %}
```

- [ ] **Step 2 : Commit**

```bash
git add templates/homepage/index.html.twig
git commit -m "style: add Stellar.ai navigation to homepage"
```

---

## Task 3 : Section Hero

**Files:**
- Modify: `templates/homepage/index.html.twig`

- [ ] **Step 1 : Ajouter le CSS Hero dans le bloc `<style>` existant (avant `</style>`)**

```css
  /* ── HERO ── */
  .hero-section {
    padding: 96px 0 128px;
    text-align: center;
  }
  .hero-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 32px;
    font-size: 14px;
    font-weight: 500;
    color: #111827;
  }
  .badge-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 24px;
    height: 24px;
    border: 1px solid #d1d5db;
    border-radius: 6px;
  }
  .hero-heading {
    font-size: clamp(48px, 6vw, 80px);
    font-weight: 400;
    line-height: 1.1;
    letter-spacing: -0.02em;
    margin-bottom: 20px;
    color: #111827;
  }
  .gradient-text {
    background: linear-gradient(to right, #111827, #9ca3af, #d1d5db);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .hero-sub {
    font-size: 18px;
    color: #6b7280;
    max-width: 672px;
    margin: 0 auto 32px;
    line-height: 1.6;
  }
  .hero-ctas {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 24px;
    flex-wrap: wrap;
  }
  .hero-link {
    font-size: 15px;
    color: #111827;
    text-decoration: underline;
    text-underline-offset: 3px;
  }
  .hero-link:hover { color: #374151; }
```

- [ ] **Step 2 : Ajouter le HTML Hero après le `</nav>` et avant `{% endblock %}`**

```twig
<!-- HERO -->
<section class="hero-section">
  <div class="page-wrapper">

    <div class="hero-badge animate-fade-in-up" style="opacity:0;animation-delay:0.2s">
      <span class="badge-icon">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="#111827"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>
      </span>
      FrankenPHP · Worker Mode · Early Hints
    </div>

    <h1 class="hero-heading animate-fade-in-up" style="opacity:0;animation-delay:0.3s">
      Deploy Faster. Scale Smarter.<br>
      <span class="gradient-text">PHP Powers You Up.</span>
    </h1>

    <p class="hero-sub animate-fade-in-up" style="opacity:0;animation-delay:0.4s">
      FrankenPHP + Symfony on Clever Cloud — worker mode, early hints,
      and API Platform out of the box.
    </p>

    <div class="hero-ctas animate-fade-in-up" style="opacity:0;animation-delay:0.5s">
      <a class="btn-primary" href="/api" target="_blank" rel="noopener">Open Swagger UI</a>
      <a class="hero-link" href="/api/monsters.jsonld" target="_blank" rel="noopener">Monsters API</a>
    </div>

  </div>
</section>
```

- [ ] **Step 3 : Commit**

```bash
git add templates/homepage/index.html.twig
git commit -m "style: add Stellar.ai hero section"
```

---

## Task 4 : Tab bar + conteneur vidéo

**Files:**
- Modify: `templates/homepage/index.html.twig`

- [ ] **Step 1 : Ajouter le CSS tabs + vidéo dans le bloc `<style>`**

```css
  /* ── TABS BAR ── */
  .tabs-wrapper {
    padding: 0 24px;
    max-width: 1280px;
    margin: 0 auto 16px;
  }
  .tabs-bar {
    display: inline-flex;
    background: #f3f4f6;
    border-radius: 8px;
    padding: 4px;
    width: 100%;
    justify-content: center;
  }
  .tab-btn {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 8px 20px;
    border-radius: 6px;
    border: none;
    background: transparent;
    color: #6b7280;
    font-size: 14px;
    font-family: 'Inter', sans-serif;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.15s;
    flex: 1;
    justify-content: center;
    white-space: nowrap;
  }
  .tab-btn.active {
    background: #ffffff;
    color: #111827;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  }
  .tab-sep {
    display: none;
    width: 1px;
    height: 20px;
    background: #d1d5db;
    align-self: center;
  }
  @media (min-width: 768px) {
    .tabs-bar { width: auto; }
    .tab-sep  { display: block; }
    .tab-btn  { flex: none; }
  }
  @media (max-width: 767px) {
    .tabs-bar { display: grid; grid-template-columns: 1fr 1fr; gap: 4px; }
    .tab-sep  { display: none; }
  }

  /* ── VIDEO CONTAINER ── */
  .video-section {
    padding: 0 24px;
    max-width: 1280px;
    margin: 0 auto;
  }
  .video-container {
    position: relative;
    border-radius: 24px;
    overflow: hidden;
    height: 500px;
  }
  @media (max-width: 767px) {
    .video-container { height: 400px; }
  }
  .video-container video {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }
```

- [ ] **Step 2 : Ajouter le HTML Tab bar + vidéo après `</section>` hero et avant `{% endblock %}`**

```twig
<!-- TAB BAR -->
<div class="tabs-wrapper animate-fade-in-up" style="opacity:0;animation-delay:0.6s">
  <div class="tabs-bar" id="tabs-bar">
    <button class="tab-btn active" data-tab="worker">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>
      Worker Mode
    </button>
    <span class="tab-sep"></span>
    <button class="tab-btn" data-tab="api">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/></svg>
      API Platform
    </button>
    <span class="tab-sep"></span>
    <button class="tab-btn" data-tab="perf">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>
      Performance
    </button>
    <span class="tab-sep"></span>
    <button class="tab-btn" data-tab="deploy">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4.5 16.5c-1.5 1.26-2 5-2 5s3.74-.5 5-2c.71-.84.7-2.13-.09-2.91a2.18 2.18 0 0 0-2.91-.09z"/><path d="m12 15-3-3a22 22 0 0 1 2-3.95A12.88 12.88 0 0 1 22 2c0 2.72-.78 7.5-6 11a22.35 22.35 0 0 1-4 2z"/><path d="M9 12H4s.55-3.03 2-4c1.62-1.08 5 0 5 0"/><path d="M12 15v5s3.03-.55 4-2c1.08-1.62 0-5 0-5"/></svg>
      Deploy
    </button>
  </div>
</div>

<!-- VIDEO + OVERLAYS -->
<div class="video-section animate-fade-in-up" style="opacity:0;animation-delay:0.7s">
  <div class="video-container" id="video-container">
    <video
      src="https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20260319_165750_358b1e72-c921-48b7-aaac-f200994f32fb.mp4"
      autoplay loop muted playsinline>
    </video>
    <!-- overlays injectés dans Task 5 -->
  </div>
</div>
```

- [ ] **Step 3 : Commit**

```bash
git add templates/homepage/index.html.twig
git commit -m "style: add tabs bar and video container"
```

---

## Task 5 : Overlays des 4 onglets

**Files:**
- Modify: `templates/homepage/index.html.twig`

- [ ] **Step 1 : Ajouter le CSS overlays dans le bloc `<style>`**

```css
  /* ── OVERLAYS ── */
  .overlay {
    position: absolute;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(0,0,0,0.35);
  }
  .overlay-card {
    background: #ffffff;
    border-radius: 12px;
    padding: 24px;
    width: 320px;
    box-shadow: 0 20px 60px rgba(0,0,0,0.3);
  }
  .overlay-title {
    font-size: 15px;
    font-weight: 600;
    color: #111827;
    margin-bottom: 16px;
  }
  .overlay-progress-track {
    height: 6px;
    background: #f3f4f6;
    border-radius: 9999px;
    overflow: hidden;
    margin-bottom: 16px;
  }
  .overlay-progress-fill {
    height: 100%;
    border-radius: 9999px;
    transition: width 0.4s ease;
  }
  .overlay-steps {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .overlay-step {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 13px;
    color: #6b7280;
  }
  .step-icon {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    font-size: 11px;
  }
  .step-done  { background: #d1fae5; color: #059669; }
  .step-spin  { background: #ede9fe; color: #7c3aed; }
  .step-wait  { background: #f3f4f6; color: #9ca3af; }
  .method-badge {
    display: inline-flex;
    font-size: 10px;
    font-weight: 700;
    padding: 2px 6px;
    border-radius: 4px;
    font-family: monospace;
    margin-right: 4px;
  }
  .m-get    { background: #d1fae5; color: #059669; }
  .m-post   { background: #dbeafe; color: #1d4ed8; }
  .m-delete { background: #fee2e2; color: #dc2626; }
  .metric-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 6px 0;
    border-bottom: 1px solid #f3f4f6;
    font-size: 13px;
  }
  .metric-row:last-child { border-bottom: none; }
  .metric-val {
    font-weight: 600;
    color: #111827;
    font-variant-numeric: tabular-nums;
  }
  .badge-success {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    background: #d1fae5;
    color: #059669;
    font-size: 11px;
    font-weight: 600;
    padding: 3px 8px;
    border-radius: 9999px;
  }
  .checklist-item {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 13px;
    color: #374151;
    padding: 5px 0;
  }
  .check-circle {
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: #d1fae5;
    color: #059669;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 10px;
    flex-shrink: 0;
  }
  .overlay-btn {
    display: block;
    width: 100%;
    margin-top: 16px;
    padding: 10px;
    background: #111827;
    color: #ffffff;
    border: none;
    border-radius: 9999px;
    font-size: 13px;
    font-weight: 500;
    font-family: 'Inter', sans-serif;
    cursor: pointer;
    text-align: center;
    text-decoration: none;
    transition: background 0.15s;
  }
  .overlay-btn:hover { background: #374151; color: #ffffff; }
```

- [ ] **Step 2 : Remplacer le commentaire `<!-- overlays injectés dans Task 5 -->` par les 4 overlays**

Localiser `<!-- overlays injectés dans Task 5 -->` dans `templates/homepage/index.html.twig` et remplacer par :

```twig
    <!-- Overlay: Worker Mode -->
    <div class="overlay animate-fade-in-overlay" data-overlay="worker">
      <div class="overlay-card animate-slide-up-overlay">
        <div class="overlay-title">FrankenPHP Worker</div>
        <div class="overlay-progress-track">
          <div class="overlay-progress-fill" style="width:25%;background:#7c3aed;"></div>
        </div>
        <div class="overlay-steps">
          <div class="overlay-step">
            <span class="step-icon step-done">✓</span> Boot
          </div>
          <div class="overlay-step">
            <span class="step-icon step-done">✓</span> Warm
          </div>
          <div class="overlay-step">
            <span class="step-icon step-spin">●</span> Ready
          </div>
          <div class="overlay-step">
            <span class="step-icon step-wait">○</span> Serving
          </div>
        </div>
      </div>
    </div>

    <!-- Overlay: API Platform -->
    <div class="overlay animate-fade-in-overlay" data-overlay="api" style="display:none">
      <div class="overlay-card animate-slide-up-overlay">
        <div class="overlay-title">API Platform</div>
        <div class="overlay-progress-track">
          <div class="overlay-progress-fill" style="width:67%;background:#f97316;"></div>
        </div>
        <div class="overlay-steps">
          <div class="overlay-step">
            <span class="method-badge m-get">GET</span> /api/monsters
          </div>
          <div class="overlay-step">
            <span class="method-badge m-post">POST</span> /api/monsters
          </div>
          <div class="overlay-step">
            <span class="method-badge m-get">GET</span> /api/monsters/{id}
          </div>
          <div class="overlay-step">
            <span class="method-badge m-delete">DEL</span> /api/monsters/{id}
          </div>
        </div>
      </div>
    </div>

    <!-- Overlay: Performance -->
    <div class="overlay animate-fade-in-overlay" data-overlay="perf" style="display:none">
      <div class="overlay-card animate-slide-up-overlay">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;">
          <div class="overlay-title" style="margin:0">Benchmark Results</div>
          <span class="badge-success">✓ Passed</span>
        </div>
        <div class="metric-row">
          <span style="color:#6b7280">Throughput</span>
          <span class="metric-val">12 400 req/s</span>
        </div>
        <div class="metric-row">
          <span style="color:#6b7280">Latency P99</span>
          <span class="metric-val">< 2 ms</span>
        </div>
        <div class="metric-row">
          <span style="color:#6b7280">Errors</span>
          <span class="metric-val" style="color:#059669">0</span>
        </div>
        <div class="metric-row">
          <span style="color:#6b7280">Worker Mode</span>
          <span class="metric-val" style="color:#7c3aed">ON</span>
        </div>
      </div>
    </div>

    <!-- Overlay: Deploy -->
    <div class="overlay animate-fade-in-overlay" data-overlay="deploy" style="display:none">
      <div class="overlay-card animate-slide-up-overlay">
        <div class="overlay-title">Deploy to Clever Cloud</div>
        <div class="checklist-item">
          <span class="check-circle">✓</span> Dockerfile detected
        </div>
        <div class="checklist-item">
          <span class="check-circle">✓</span> FrankenPHP runtime
        </div>
        <div class="checklist-item">
          <span class="check-circle">✓</span> Environment variables set
        </div>
        <div class="checklist-item">
          <span class="check-circle">✓</span> Health check passing
        </div>
        <a class="overlay-btn" href="https://console.clever-cloud.com" target="_blank" rel="noopener">
          View on Clever Cloud
        </a>
      </div>
    </div>
```

- [ ] **Step 3 : Commit**

```bash
git add templates/homepage/index.html.twig
git commit -m "style: add four overlay cards for tab section"
```

---

## Task 6 : Vanilla JS — cycling des onglets

**Files:**
- Modify: `templates/homepage/index.html.twig`

- [ ] **Step 1 : Ajouter le bloc `<script>` juste avant `{% endblock %}` en fin de fichier**

```twig
<script>
(function () {
  var tabs   = ['worker', 'api', 'perf', 'deploy'];
  var current = 0;
  var interval;

  function setTab(name) {
    // Mise à jour des boutons
    document.querySelectorAll('[data-tab]').forEach(function (btn) {
      if (btn.dataset.tab === name) {
        btn.classList.add('active');
      } else {
        btn.classList.remove('active');
      }
    });

    // Affichage des overlays
    document.querySelectorAll('[data-overlay]').forEach(function (el) {
      if (el.dataset.overlay === name) {
        el.style.display = '';
        // Re-trigger animation
        el.classList.remove('animate-fade-in-overlay');
        void el.offsetWidth;
        el.classList.add('animate-fade-in-overlay');
      } else {
        el.style.display = 'none';
      }
    });
  }

  function startCycle() {
    return setInterval(function () {
      current = (current + 1) % tabs.length;
      setTab(tabs[current]);
    }, 4000);
  }

  // Click handlers
  document.querySelectorAll('[data-tab]').forEach(function (btn) {
    btn.addEventListener('click', function () {
      current = tabs.indexOf(btn.dataset.tab);
      setTab(tabs[current]);
      clearInterval(interval);
      interval = startCycle();
    });
  });

  // Init
  setTab(tabs[0]);
  interval = startCycle();
})();
</script>
```

- [ ] **Step 2 : Commit**

```bash
git add templates/homepage/index.html.twig
git commit -m "feat: add vanilla JS tab cycling with 4s interval"
```

---

## Task 7 : Bande de logos + Footer

**Files:**
- Modify: `templates/homepage/index.html.twig`

- [ ] **Step 1 : Ajouter le CSS logos + footer dans le bloc `<style>`**

```css
  /* ── LOGOS ── */
  .logos-section {
    padding: 96px 24px 0;
    max-width: 1280px;
    margin: 0 auto;
  }
  .logos-row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 48px;
    flex-wrap: wrap;
  }
  .logo-text {
    color: #9ca3af;
    user-select: none;
    transition: color 0.15s;
  }
  .logo-text:hover { color: #6b7280; }
  .logo-franken {
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
  }
  .logo-symfony {
    font-size: 16px;
    font-style: italic;
    font-family: Georgia, serif;
  }
  .logo-cc {
    font-size: 13px;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .logo-api {
    font-size: 16px;
    font-style: italic;
    font-family: Georgia, serif;
    font-weight: 700;
  }
  .logo-caddy {
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    font-family: monospace;
  }
  .logo-docker {
    font-size: 14px;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  /* ── FOOTER ── */
  .footer {
    margin-top: 48px;
    border-top: 1px solid #f0f0f0;
    padding: 22px 24px 40px;
    max-width: 1280px;
    margin-left: auto;
    margin-right: auto;
  }
  .footer-links {
    display: flex;
    flex-wrap: wrap;
    gap: 16px;
    justify-content: center;
    margin-bottom: 12px;
  }
  .footer-links a {
    font-size: 13px;
    color: #9ca3af;
    transition: color 0.15s;
  }
  .footer-links a:hover { color: #111827; }
  .footer-copy {
    text-align: center;
    font-size: 11px;
    color: #d1d5db;
  }
```

- [ ] **Step 2 : Ajouter le HTML logos + footer après `</div>` de `.video-section` et avant `<script>`**

```twig
<!-- LOGOS -->
<section class="logos-section animate-fade-in-up" style="opacity:0;animation-delay:0.8s">
  <div class="logos-row">
    <span class="logo-text logo-franken">FrankenPHP</span>
    <span class="logo-text logo-symfony">Symfony</span>
    <span class="logo-text logo-cc">
      <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 10h-1.26A8 8 0 1 0 9 20h9a5 5 0 0 0 0-10z"/></svg>
      Clever Cloud
    </span>
    <span class="logo-text logo-api">API Platform</span>
    <span class="logo-text logo-caddy">Caddy</span>
    <span class="logo-text logo-docker">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M8 12h8M12 8v8"/></svg>
      Docker
    </span>
  </div>
</section>

<!-- FOOTER -->
<footer class="footer">
  <div class="footer-links">
    <a href="https://www.clever-cloud.com" target="_blank" rel="noopener">clever-cloud.com</a>
    <a href="https://developers.clever-cloud.com/doc/applications/php/" target="_blank" rel="noopener">Docs PHP</a>
    <a href="https://frankenphp.dev/docs/" target="_blank" rel="noopener">FrankenPHP</a>
    <a href="https://symfony.com/doc/current/index.html" target="_blank" rel="noopener">Symfony</a>
    <a href="https://api-platform.com/docs/" target="_blank" rel="noopener">API Platform</a>
    <a href="https://academy.clever.cloud/" target="_blank" rel="noopener">Certification CC</a>
  </div>
  <p class="footer-copy">Open source demo &middot; Deployed on Clever Cloud</p>
</footer>
```

- [ ] **Step 3 : Commit**

```bash
git add templates/homepage/index.html.twig
git commit -m "style: add tech logos strip and footer"
```

---

## Task 8 : Push Clever Cloud + vérification

**Files:** aucun

- [ ] **Step 1 : Vérifier l'état git avant push**

```bash
cd /Users/vitio/ClaudeByVitio/demo-php-frankenphp
git log --oneline -6
git status
```

Résultat attendu : working tree clean, 6+ commits depuis le redesign.

- [ ] **Step 2 : Push vers Clever Cloud**

```bash
git push origin main
```

Résultat attendu : push accepté, déploiement déclenché automatiquement.

- [ ] **Step 3 : Vérifier le déploiement**

Ouvrir l'URL de l'application sur la console Clever Cloud (section "Overview" de l'app demo-php-frankenphp).

Vérifications visuelles :
- [ ] Fond blanc (pas sombre)
- [ ] Navigation sticky Inter, bouton "Open Swagger UI" noir rounded-full
- [ ] Hero : heading gradient, badge éclair
- [ ] Tabs : 4 boutons, auto-cycle 4s visible
- [ ] Vidéo en fond avec overlay Worker Mode par défaut
- [ ] Clic sur chaque tab → overlay correct
- [ ] Logos strip + footer visible en bas de page
- [ ] Responsive mobile : tabs en 2×2, nav sans center links

---

## Résultat final attendu

Page blanche, Inter, navigation sticky, hero animé avec gradient text `"PHP Powers You Up."`, section tabs interactive auto-cycle 4s avec vidéo CloudFront en fond et 4 overlays contextuels (Worker Mode / API Platform / Performance / Deploy), bande de 6 logos tech, footer minimal. Déployé sur Clever Cloud via simple `git push`.
