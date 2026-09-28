# Frontend-for-Mausam-V2
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>IMD Mausam v2 — India Meteorological Department</title>
  <style>
    :root {
      /* Official IMD Mausam Color Theme */
      --imd-navy: #0b2545;
      --imd-blue: #134074;
      --imd-accent: #0077b6;
      --imd-gold: #ffb703;
      --imd-bg: #eef4f8;
      --card-bg: #ffffff;
      
      --text-main: #0b132b;
      --text-muted: #5c6b73;
      --border-color: #d0dbdf;
      
      /* Invariant Warning & Severity Colors */
      --alert-red: #d90429;
      --alert-red-bg: #ffccd5;
      --alert-orange: #fb8500;
      
      /* Persona Accent Indicators */
      --badge-health: #e63946;
      --badge-farmer: #2a9d8f;
      --badge-commuter: #0077b6;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
      background-color: var(--imd-bg);
      color: var(--text-main);
      padding-bottom: 3rem;
    }

    /* Official IMD Top App Bar */
    .imd-app-header {
      background: linear-gradient(135deg, var(--imd-navy) 0%, var(--imd-blue) 100%);
      color: #ffffff;
      padding: 0.9rem 1.2rem;
      box-shadow: 0 3px 10px rgba(0, 0, 0, 0.15);
    }

    .header-content {
      max-width: 900px;
      margin: 0 auto;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 0.75rem;
    }

    .brand-cluster {
      display: flex;
      align-items: center;
      gap: 0.75rem;
    }

    .emblem-circle {
      width: 44px;
      height: 44px;
      background: #ffffff;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.3rem;
      border: 2px solid var(--imd-gold);
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    }

    .brand-titles h1 {
      font-size: 1.25rem;
      font-weight: 800;
      letter-spacing: 0.5px;
      text-transform: uppercase;
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .v2-pill {
      background: var(--imd-gold);
      color: var(--imd-navy);
      font-size: 0.65rem;
      font-weight: 800;
      padding: 2px 7px;
      border-radius: 12px;
      letter-spacing: 0.5px;
    }

    .brand-titles p {
      font-size: 0.75rem;
      color: #cbd5e1;
    }

    .lang-switcher select {
      background: rgba(255, 255, 255, 0.15);
      color: #ffffff;
      border: 1px solid rgba(255, 255, 255, 0.3);
      padding: 0.35rem 0.65rem;
      border-radius: 6px;
      font-size: 0.82rem;
      outline: none;
      cursor: pointer;
    }

    .lang-switcher select option {
      background: var(--imd-navy);
      color: #ffffff;
    }

    /* Main Container */
    .app-viewport {
      max-width: 900px;
      margin: 1.25rem auto 0;
      padding: 0 1rem;
    }

    /* Updated Station Search Bar */
    .search-strip {
      background: var(--card-bg);
      border-radius: 30px;
      padding: 0.35rem 0.5rem 0.35rem 1.2rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      box-shadow: 0 2px 8px rgba(0,0,0,0.06);
      border: 1px solid var(--border-color);
      margin-bottom: 1rem;
    }

    .search-strip input {
      flex: 1;
      border: none;
      outline: none;
      font-size: 0.95rem;
      color: var(--text-main);
      background: transparent;
    }

    .search-strip button {
      background: var(--imd-blue);
      color: #ffffff;
      border: none;
      padding: 0.55rem 1.3rem;
      border-radius: 25px;
      font-size: 0.85rem;
      font-weight: 700;
      cursor: pointer;
      transition: background 0.15s ease;
    }

    .search-strip button:hover {
      background: var(--imd-navy);
    }

    /* Persona Segmented Bar (The v2 Modernization Layer) */
    .persona-bar-card {
      background: var(--card-bg);
      border-radius: 12px;
      padding: 0.75rem 1rem;
      box-shadow: 0 2px 6px rgba(0,0,0,0.04);
      border: 1px solid var(--border-color);
      margin-bottom: 1.25rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 0.75rem;
    }

    .persona-label-group {
      display: flex;
      flex-direction: column;
    }

    .persona-label-group span:first-child {
      font-size: 0.75rem;
      text-transform: uppercase;
      font-weight: 800;
      color: var(--text-muted);
      letter-spacing: 0.5px;
    }

    .persona-label-group span:last-child {
      font-size: 0.85rem;
      font-weight: 700;
      color: var(--imd-blue);
    }

    .persona-chips {
      display: flex;
      gap: 0.4rem;
      flex-wrap: wrap;
    }

    .chip-btn {
      background: var(--imd-bg);
      border: 1px solid var(--border-color);
      color: var(--text-muted);
      padding: 0.45rem 0.85rem;
      border-radius: 20px;
      font-size: 0.8rem;
      font-weight: 700;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 0.35rem;
      transition: all 0.15s ease;
    }

    .chip-btn.active[data-persona="health"] {
      background: var(--badge-health);
      color: #ffffff;
      border-color: var(--badge-health);
    }

    .chip-btn.active[data-persona="farmer"] {
      background: var(--badge-farmer);
      color: #ffffff;
      border-color: var(--badge-farmer);
    }

    .chip-btn.active[data-persona="commuter"] {
      background: var(--badge-commuter);
      color: #ffffff;
      border-color: var(--badge-commuter);
    }

    /* Permanent Severe Alert Invariant (IMD Red/Orange Alert) */
    .imd-alert-strip {
      background: var(--alert-red-bg);
      border-left: 6px solid var(--alert-red);
      color: var(--alert-red);
      padding: 0.9rem 1.1rem;
      border-radius: 8px;
      margin-bottom: 1.25rem;
      box-shadow: 0 2px 6px rgba(217, 4, 41, 0.15);
    }

    .imd-alert-strip h3 {
      font-size: 1rem;
      font-weight: 800;
      display: flex;
      align-items: center;
      gap: 0.4rem;
      margin-bottom: 0.2rem;
    }

    .imd-alert-strip p {
      font-size: 0.85rem;
      font-weight: 600;
      line-height: 1.4;
    }

    /* Existing Mausam App Signature: Weather Hero Summary Card */
    .imd-hero-card {
      background: linear-gradient(135deg, #ffffff 0%, #f1f7fa 100%);
      border: 1px solid var(--border-color);
      border-radius: 14px;
      padding: 1.25rem 1.5rem;
      margin-bottom: 1.25rem;
      box-shadow: 0 4px 12px rgba(0,0,0,0.05);
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 1rem;
    }

    .hero-station-meta h2 {
      font-size: 1.45rem;
      font-weight: 800;
      color: var(--imd-navy);
    }

    .hero-station-meta p {
      font-size: 0.85rem;
      color: var(--text-muted);
      margin-top: 0.15rem;
    }

    .hero-nowcast-pill {
      display: inline-block;
      margin-top: 0.5rem;
      background: #e0f2fe;
      color: var(--imd-accent);
      padding: 0.2rem 0.65rem;
      border-radius: 12px;
      font-size: 0.75rem;
      font-weight: 700;
    }

    .hero-temp-display {
      display: flex;
      align-items: center;
      gap: 0.75rem;
    }

    .hero-temp-ring {
      font-size: 3rem;
      font-weight: 900;
      color: var(--imd-navy);
      line-height: 1;
    }

    .hero-submetrics {
      display: flex;
      flex-direction: column;
      gap: 0.2rem;
      font-size: 0.8rem;
      font-weight: 600;
      color: var(--text-muted);
      border-left: 2px solid var(--border-color);
      padding-left: 0.75rem;
    }

    /* Section Label */
    .section-label {
      font-size: 0.75rem;
      font-weight: 800;
      text-transform: uppercase;
      letter-spacing: 0.75px;
      color: var(--text-muted);
      margin: 0.5rem 0 0.75rem 0.25rem;
    }

    /* Ranked Dynamic Widgets Stack */
    .adaptive-deck {
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }

    .deck-card {
      background: var(--card-bg);
      border: 1px solid var(--border-color);
      border-radius: 12px;
      padding: 1.1rem 1.25rem;
      box-shadow: 0 2px 6px rgba(0,0,0,0.04);
      transition: transform 0.15s ease, box-shadow 0.15s ease;
    }

    .deck-card.top-priority {
      border-left: 6px solid var(--imd-accent);
      box-shadow: 0 4px 14px rgba(0, 119, 182, 0.12);
    }

    .deck-card-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 0.85rem;
    }

    .card-title-group h4 {
      font-size: 1.05rem;
      font-weight: 800;
      color: var(--imd-navy);
    }

    .rank-tag {
      font-size: 0.7rem;
      font-weight: 800;
      text-transform: uppercase;
      padding: 0.2rem 0.55rem;
      border-radius: 4px;
      background: var(--imd-bg);
      color: var(--text-muted);
      border: 1px solid var(--border-color);
    }

    .top-priority .rank-tag {
      background: #e0f2fe;
      color: var(--imd-accent);
      border-color: #bae6fd;
    }

    .grid-measurements {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
      gap: 0.75rem;
      margin-bottom: 0.85rem;
    }

    .meas-box {
      background: var(--imd-bg);
      padding: 0.65rem 0.85rem;
      border-radius: 8px;
      border: 1px solid #e2e8f0;
    }

    .meas-label {
      font-size: 0.7rem;
      font-weight: 700;
      color: var(--text-muted);
      text-transform: uppercase;
      letter-spacing: 0.25px;
    }

    .meas-val {
      font-size: 1.25rem;
      font-weight: 800;
      color: var(--imd-navy);
      margin-top: 0.15rem;
    }

    .why-tag {
      font-size: 0.78rem;
      color: var(--text-muted);
      border-top: 1px dashed var(--border-color);
      padding-top: 0.5rem;
      display: flex;
      align-items: center;
      gap: 0.35rem;
      font-weight: 500;
    }

    .state-message {
      background: var(--card-bg);
      border: 1px solid var(--border-color);
      border-radius: 12px;
      padding: 2.5rem 1rem;
      text-align: center;
      color: var(--text-muted);
      font-weight: 600;
    }

    .state-message.error {
      background: var(--alert-red-bg);
      color: var(--alert-red);
      border-color: #fca5a5;
    }
  </style>
</head>
<body>

  <!-- Official IMD Top Banner -->
  <header class="imd-app-header">
    <div class="header-content">
      <div class="brand-cluster">
        <div class="emblem-circle" title="Government of India Emblem">🇮🇳</div>
        <div class="brand-titles">
          <h1>
            <span>मौसम MAUSAM</span>
            <span class="v2-pill">v2.0 ADAPTIVE</span>
          </h1>
          <p id="txt-subhead">India Meteorological Department | Ministry of Earth Sciences</p>
        </div>
      </div>

      <div class="lang-switcher">
        <select id="lang-select" onchange="switchLanguage(this.value)">
          <option value="en">English (EN)</option>
          <option value="hi">हिन्दी (HI)</option>
          <option value="ta">தமிழ் (TA)</option>
        </select>
      </div>
    </div>
  </header>

  <div class="app-viewport">

    <!-- Search Station Bar -->
    <div class="search-strip">
      <span>🔍</span>
      <input 
        type="text" 
        id="station-input" 
        placeholder="Search District / Station (e.g. Chennai, Pune, Shimla, Puri)..." 
        value="Chennai"
        onkeydown="if(event.key==='Enter') executeUpdate()"
      />
      <button id="btn-fetch" onclick="executeUpdate()">GO</button>
    </div>

    <!-- Segmented Persona Selector (The Smart v2 Module) -->
    <div class="persona-bar-card">
      <div class="persona-label-group">
        <span id="txt-viewing-mode">HOMEPAGE PROFILE</span>
        <span id="txt-active-mode-name">Health & Environment</span>
      </div>

      <div class="persona-chips">
        <button class="chip-btn active" data-persona="health" onclick="selectPersona('health')">
          <span>🏥</span> <span id="lbl-p-health">Health (AQI)</span>
        </button>
        <button class="chip-btn" data-persona="farmer" onclick="selectPersona('farmer')">
          <span>🌾</span> <span id="lbl-p-farmer">Agromet (Farmer)</span>
        </button>
        <button class="chip-btn" data-persona="commuter" onclick="selectPersona('commuter')">
          <span>🚗</span> <span id="lbl-p-commuter">Commuter</span>
        </button>
      </div>
    </div>

    <!-- Invariant Severe Warning Strip (Pinned to Top) -->
    <div id="severe-alert-anchor" aria-live="assertive"></div>

    <!-- Signature IMD Weather Hero Overview -->
    <div class="imd-hero-card" id="hero-card" style="display: none;">
      <div class="hero-station-meta">
        <h2 id="hero-city-name">--</h2>
        <p id="hero-state-name">India</p>
        <span class="hero-nowcast-pill" id="hero-nowcast-summary">Station Telemetry Active</span>
      </div>
      <div class="hero-temp-display">
        <div class="hero-temp-ring" id="hero-temp-val">--°</div>
        <div class="hero-submetrics">
          <span id="hero-humidity">Humidity: --%</span>
          <span id="hero-uv">UV Index: --</span>
          <span id="hero-rain">Rain Risk: --%</span>
        </div>
      </div>
    </div>

    <!-- Re-Ranked Persona Widgets Section -->
    <div class="section-label" id="txt-widgets-heading">Personalized Priority Layout</div>
    <main id="deck-container" class="adaptive-deck"></main>

  </div>

  <script>
    const BACKEND_URL = "http://127.0.0.1:8000";
    let activePersona = "health";
    let activeLang = "en";

    const i18nDict = {
      en: {
        subhead: "India Meteorological Department | Ministry of Earth Sciences",
        searchPlaceholder: "Search District / Station (e.g. Chennai, Pune, Shimla, Puri)...",
        goBtn: "GO",
        viewingMode: "HOMEPAGE PROFILE",
        healthBtn: "Health (AQI)",
        farmerBtn: "Agromet (Farmer)",
        commuterBtn: "Commuter",
        widgetsHeading: "Personalized Priority Layout",
        loading: "Fetching official IMD & CAAQMS telemetry...",
        topPriority: "PRIORITY #1 (ACTIVE INTENT)",
        rankPrefix: "RANK #",
        whyPrefix: "Why am I seeing this first? "
      },
      hi: {
        subhead: "भारत मौसम विज्ञान विभाग | पृथ्वी विज्ञान मंत्रालय",
        searchPlaceholder: "ज़िला या मौसम केंद्र खोजें (उदा. चेन्नई, पुणे, शिमला, पुरी)...",
        goBtn: "खोजें",
        viewingMode: "होमपेज प्रोफ़ाइल",
        healthBtn: "स्वास्थ्य (AQI)",
        farmerBtn: "कृषि मौसम (किसान)",
        commuterBtn: "यात्री (दैनिक)",
        widgetsHeading: "प्राथमिकता-आधारित वैयक्तिकृत लेआउट",
        loading: "आधिकारिक मौसम डेटा लोड हो रहा है...",
        topPriority: "प्राथमिकता #1 (सक्रिय प्रोफ़ाइल)",
        rankPrefix: "क्रमांक #",
        whyPrefix: "यह कार्ड पहले क्यों दिख रहा है? "
      },
      ta: {
        subhead: "இந்திய வானிலை ஆய்வு மையம் | புவி அறிவியல் அமைச்சகம்",
        searchPlaceholder: "மாவட்டம் / வானிலை மையம் தேடுங்கள் (எ.கா. சென்னை, புனே)...",
        goBtn: "தேடு",
        viewingMode: "முகப்புப் பக்க சுயவிவரம்",
        healthBtn: "சுகாதாரம் (AQI)",
        farmerBtn: "வேளாண் வானிலை",
        commuterBtn: "பயணிகள்",
        widgetsHeading: "முன்னுரிமை அடிப்படையிலான தகவல் தொகுப்பு",
        loading: "வானிலை தகவல்கள் பெறப்படுகின்றன...",
        topPriority: "முன்னுரிமை #1 (தேர்ந்தெடுக்கப்பட்டது)",
        rankPrefix: "வரிசை #",
        whyPrefix: "முன்னுரிமை விளக்கம்: "
      }
    };

    function switchLanguage(lang) {
      activeLang = lang;
      const t = i18nDict[lang];
      document.getElementById("txt-subhead").textContent = t.subhead;
      document.getElementById("station-input").placeholder = t.searchPlaceholder;
      document.getElementById("btn-fetch").textContent = t.goBtn;
      document.getElementById("txt-viewing-mode").textContent = t.viewingMode;
      document.getElementById("lbl-p-health").textContent = t.healthBtn;
      document.getElementById("lbl-p-farmer").textContent = t.farmerBtn;
      document.getElementById("lbl-p-commuter").textContent = t.commuterBtn;
      document.getElementById("txt-widgets-heading").textContent = t.widgetsHeading;
      executeUpdate();
    }

    function selectPersona(persona) {
      activePersona = persona;
      document.querySelectorAll(".chip-btn").forEach(btn => {
        btn.classList.toggle("active", btn.dataset.persona === persona);
      });
      const names = { health: "Health & Environment", farmer: "Agromet / Farmer", commuter: "Commuter / Travel" };
      document.getElementById("txt-active-mode-name").textContent = names[persona];
      executeUpdate();
    }

    async function executeUpdate() {
      const city = document.getElementById("station-input").value.trim() || "Chennai";
      const deckContainer = document.getElementById("deck-container");
      const alertAnchor = document.getElementById("severe-alert-anchor");
      const heroCard = document.getElementById("hero-card");

      deckContainer.innerHTML = `<div class="state-message">${i18nDict[activeLang].loading}</div>`;
      alertAnchor.innerHTML = "";

      try {
        const res = await fetch(`${BACKEND_URL}/api/dashboard?city=${encodeURIComponent(city)}&persona=${encodeURIComponent(activePersona)}`);
        if (!res.ok) {
          const err = await res.json().catch(() => ({}));
          throw new Error(err.detail || `Server status ${res.status}`);
        }

        const data = await res.json();

        // 1. Fill IMD Hero Summary Card
        heroCard.style.display = "flex";
        document.getElementById("hero-city-name").textContent = data.city_name;
        document.getElementById("hero-state-name").textContent = `${data.state}, India`;
        
        // Extract common primary telemetry for hero ring
        const commuterW = data.widgets.find(w => w.widget_id === "commuter_card");
        const healthW = data.widgets.find(w => w.widget_id === "health_card");
        
        if (commuterW) {
          document.getElementById("hero-temp-val").textContent = commuterW.data["Temperature"] || "30°C";
          document.getElementById("hero-rain").textContent = `Rain Risk: ${commuterW.data["Precipitation Risk"] || "0%"}`;
        }
        if (healthW) {
          document.getElementById("hero-humidity").textContent = `Humidity: ${healthW.data["Humidity"] || "--"}`;
          document.getElementById("hero-uv").textContent = `UV Index: ${healthW.data["UV Index"] || "--"}`;
        }

        // 2. Safety Invariant Banner (Permanent Top Container)
        if (data.active_alerts && data.active_alerts.length > 0) {
          data.active_alerts.forEach(alert => {
            const el = document.createElement("div");
            el.className = "imd-alert-strip";
            el.innerHTML = `
              <h3>⚠️ IMD SEVERE WARNING: ${alert.title} [${alert.severity_level}]</h3>
              <p>${alert.description}</p>
            `;
            alertAnchor.appendChild(el);
          });
        }

        // 3. Render Ranked Deck
        deckContainer.innerHTML = "";
        data.widgets.forEach((widget, index) => {
          const isTop = index === 0;
          const card = document.createElement("section");
          card.className = `deck-card ${isTop ? "top-priority" : ""}`;

          let tiles = "";
          for (const [k, v] of Object.entries(widget.data)) {
            tiles += `
              <div class="meas-box">
                <div class="meas-label">${k}</div>
                <div class="meas-val">${v}</div>
              </div>`;
          }

          const rankBadgeText = isTop 
            ? i18nDict[activeLang].topPriority 
            : `${i18nDict[activeLang].rankPrefix}${index + 1}`;

          card.innerHTML = `
            <div class="deck-card-header">
              <div class="card-title-group">
                <h4>${widget.title}</h4>
