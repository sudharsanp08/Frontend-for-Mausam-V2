# Frontend-for-Mausam-V2
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>IMD Mausam v2 — India Meteorological Department</title>
  
  <!-- Leaflet CSS for Interactive India Map -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />

  <style>
    :root {
      --imd-navy: #082545;
      --imd-blue: #134074;
      --imd-cyan: #0077b6;
      --imd-gold: #ffb703;
      --imd-bg: #edf2f7;
      --card-bg: #ffffff;
      
      --text-main: #0b132b;
      --text-muted: #4a5568;
      --border-color: #cbd5e1;
      
      --alert-red: #d90429;
      --alert-red-bg: #fee2e2;
      --alert-orange: #f59e0b;
      
      --badge-health: #e63946;
      --badge-farmer: #16a34a;
      --badge-commuter: #0284c7;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
      background-color: var(--imd-bg);
      color: var(--text-main);
      padding-bottom: 3rem;
    }

    /* Official IMD App Header */
    .imd-header {
      background: linear-gradient(135deg, var(--imd-navy) 0%, var(--imd-blue) 100%);
      color: #ffffff;
      padding: 0.85rem 1.25rem;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }

    .header-inner {
      max-width: 980px;
      margin: 0 auto;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 0.75rem;
    }

    .brand-section {
      display: flex;
      align-items: center;
      gap: 0.75rem;
    }

    .emblem-badge {
      width: 44px;
      height: 44px;
      background: #ffffff;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.35rem;
      border: 2px solid var(--imd-gold);
    }

    .brand-section h1 {
      font-size: 1.25rem;
      font-weight: 800;
      letter-spacing: 0.5px;
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
      border-radius: 10px;
    }

    .brand-section p {
      font-size: 0.75rem;
      color: #cbd5e1;
    }

    .lang-switcher select {
      background: rgba(255, 255, 255, 0.15);
      color: #ffffff;
      border: 1px solid rgba(255, 255, 255, 0.35);
      padding: 0.35rem 0.65rem;
      border-radius: 6px;
      font-size: 0.85rem;
      cursor: pointer;
    }

    .lang-switcher select option {
      background: var(--imd-navy);
      color: #ffffff;
    }

    .main-viewport {
      max-width: 980px;
      margin: 1.25rem auto 0;
      padding: 0 1rem;
    }

    /* Search & Location Bar */
    .search-strip {
      background: var(--card-bg);
      border-radius: 30px;
      padding: 0.35rem 0.6rem 0.35rem 1.2rem;
      display: flex;
      align-items: center;
      gap: 0.6rem;
      box-shadow: 0 2px 8px rgba(0,0,0,0.06);
      border: 1px solid var(--border-color);
      margin-bottom: 1.25rem;
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

    /* IMD India Map Section */
    .map-card-wrapper {
      background: var(--card-bg);
      border: 1px solid var(--border-color);
      border-radius: 12px;
      overflow: hidden;
      margin-bottom: 1.25rem;
      box-shadow: 0 4px 14px rgba(0,0,0,0.06);
    }

    .map-header-tabs {
      background: #f1f5f9;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid var(--border-color);
      padding: 0.5rem 1rem;
      flex-wrap: wrap;
      gap: 0.5rem;
    }

    .map-title {
      font-size: 0.85rem;
      font-weight: 800;
      color: var(--imd-navy);
      display: flex;
      align-items: center;
      gap: 0.4rem;
      text-transform: uppercase;
    }

    .map-layer-toggles {
      display: flex;
      gap: 0.35rem;
    }

    .map-btn {
      background: #ffffff;
      border: 1px solid var(--border-color);
      font-size: 0.75rem;
      font-weight: 700;
      padding: 0.3rem 0.75rem;
      border-radius: 20px;
      cursor: pointer;
      color: var(--text-muted);
      transition: all 0.15s ease;
    }

    .map-btn.active {
      background: var(--imd-blue);
      color: #ffffff;
      border-color: var(--imd-blue);
    }

    #india-map {
      height: 380px;
      width: 100%;
      z-index: 1;
    }

    /* Permanent Severe Alert Banner */
    .alert-banner-box {
      background: var(--alert-red-bg);
      border-left: 6px solid var(--alert-red);
      color: var(--alert-red);
      padding: 0.9rem 1.1rem;
      border-radius: 8px;
      margin-bottom: 1.25rem;
      box-shadow: 0 2px 6px rgba(217, 4, 41, 0.15);
    }

    .alert-banner-box h3 {
      font-size: 1rem;
      font-weight: 800;
      margin-bottom: 0.2rem;
      display: flex;
      align-items: center;
      gap: 0.4rem;
    }

    /* IMD Current Weather Overview Card */
    .imd-overview-card {
      background: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
      border: 1px solid var(--border-color);
      border-radius: 12px;
      padding: 1.25rem 1.5rem;
      margin-bottom: 1.25rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.04);
    }

    .station-details h2 {
      font-size: 1.45rem;
      font-weight: 800;
      color: var(--imd-navy);
    }

    .station-details p {
      font-size: 0.85rem;
      color: var(--text-muted);
    }

    .station-badge {
      display: inline-block;
      margin-top: 0.4rem;
      background: #e0f2fe;
      color: var(--imd-cyan);
      padding: 0.2rem 0.65rem;
      border-radius: 12px;
      font-size: 0.75rem;
      font-weight: 700;
    }

    .temp-quadrant {
      display: flex;
      align-items: center;
      gap: 1rem;
    }

    .temp-large {
      font-size: 3.2rem;
      font-weight: 900;
      color: var(--imd-navy);
      line-height: 1;
    }

    .telemetry-mini-list {
      display: flex;
      flex-direction: column;
      gap: 0.2rem;
      font-size: 0.8rem;
      font-weight: 600;
      color: var(--text-muted);
      border-left: 2px solid var(--border-color);
      padding-left: 0.75rem;
    }

    /* v2 Persona Bar */
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

    .persona-title-col {
      display: flex;
      flex-direction: column;
    }

    .persona-title-col span:first-child {
      font-size: 0.7rem;
      text-transform: uppercase;
      font-weight: 800;
      color: var(--text-muted);
    }

    .persona-title-col span:last-child {
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
      padding: 0.4rem 0.85rem;
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

    /* Ranked Widgets Deck */
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
    }

    .deck-card.top-priority {
      border-left: 6px solid var(--imd-cyan);
      box-shadow: 0 4px 14px rgba(0, 119, 182, 0.12);
    }

    .deck-card-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 0.85rem;
    }

    .deck-card-header h4 {
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
      color: var(--imd-cyan);
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

  <!-- Official IMD App Bar -->
  <header class="imd-header">
    <div class="header-inner">
      <div class="brand-section">
        <div class="emblem-badge">🇮🇳</div>
        <div>
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

  <div class="main-viewport">

    <!-- Search Bar -->
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

    <!-- IMD Interactive India Map Section -->
    <section class="map-card-wrapper">
      <div class="map-header-tabs">
        <div class="map-title">
          <span>📡</span>
          <span id="txt-map-title">Interactive India Weather Map</span>
        </div>
        <div class="map-layer-toggles">
          <button class="map-btn active" id="btn-layer-base" onclick="setMapLayer('base')">Standard Map</button>
          <button class="map-btn" id="btn-layer-radar" onclick="setMapLayer('radar')">Radar / Clouds</button>
          <button class="map-btn" id="btn-layer-warning" onclick="setMapLayer('warning')">IMD Alert Zones</button>
        </div>
      </div>
      <div id="india-map"></div>
    </section>

    <!-- Invariant Emergency Alert Layer -->
    <div id="severe-alert-anchor" aria-live="assertive"></div>

    <!-- Current Observation Overview -->
    <section class="imd-overview-card" id="overview-card" style="display: none;">
      <div class="station-details">
        <h2 id="overview-city-name">--</h2>
        <p id="overview-state-name">India</p>
        <span class="station-badge">Live Station Telemetry</span>
      </div>
      <div class="temp-quadrant">
        <div class="temp-large" id="overview-temp">--°</div>
        <div class="telemetry-mini-list">
          <span id="overview-humidity">Humidity: --%</span>
          <span id="overview-uv">UV Index: --</span>
          <span id="overview-rain">Rain Probability: --%</span>
        </div>
      </div>
    </section>

    <!-- v2 Persona Intelligence Bar -->
    <div class="persona-bar-card">
      <div class="persona-title-col">
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

    <!-- Dynamic Ranked Widgets Deck -->
    <main id="deck-container" class="adaptive-deck"></main>

  </div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <script>
    const BACKEND_URL = "http://127.0.0.1:8000";
    let activePersona = "health";
    let activeLang = "en";
    let leafletMap = null;
    let currentMarker = null;
    let radarOverlayLayer = null;

    // Standard coordinates for Indian regional centers
    const INDIAN_COORDS = {
      "Chennai": [13.0827, 80.2707],
      "New Delhi": [28.6139, 77.2090],
      "Mumbai": [19.0760, 72.8777],
      "Kolkata": [22.5726, 88.3639],
      "Bengaluru": [12.9716, 77.5946],
      "Pune": [18.5204, 73.8567],
      "Puri": [19.8135, 85.8312],
      "Shimla": [31.1048, 77.1734],
      "Hyderabad": [17.3850, 78.4867],
      "Ahmedabad": [23.0225, 72.5714],
      "Jaipur": [26.9124, 75.7873],
      "Guwahati": [26.1445, 91.7362]
    };

    const i18nDict = {
      en: {
        subhead: "India Meteorological Department | Ministry of Earth Sciences",
        searchPlaceholder: "Search District / Station (e.g. Chennai, Pune, Shimla, Puri)...",
        goBtn: "GO",
        mapTitle: "Interactive India Weather Map",
        layerBase: "Standard Map",
        layerRadar: "Radar / Clouds",
        layerWarning: "IMD Alert Zones",
        viewingMode: "HOMEPAGE PROFILE",
        healthBtn: "Health (AQI)",
        farmerBtn: "Agromet (Farmer)",
        commuterBtn: "Commuter",
        loading: "Fetching official IMD & CAAQMS telemetry...",
        topPriority: "PRIORITY #1 (ACTIVE INTENT)",
        rankPrefix: "RANK #",
        whyPrefix: "Why am I seeing this first? "
      },
      hi: {
        subhead: "भारत मौसम विज्ञान विभाग | पृथ्वी विज्ञान मंत्रालय",
        searchPlaceholder: "ज़िला या मौसम केंद्र खोजें (उदा. चेन्नई, पुणे, शिमला, पुरी)...",
        goBtn: "खोजें",
        mapTitle: "इंटरैक्टिव भारत मौसम मानचित्र",
        layerBase: "मानक मानचित्र",
        layerRadar: "रडार / बादल",
        layerWarning: "चेतावनी क्षेत्र",
        viewingMode: "होमपेज प्रोफ़ाइल",
        healthBtn: "स्वास्थ्य (AQI)",
        farmerBtn: "कृषि मौसम (किसान)",
        commuterBtn: "यात्री (दैनिक)",
        loading: "आधिकारिक मौसम डेटा लोड हो रहा है...",
        topPriority: "प्राथमिकता #1 (सक्रिय प्रोफ़ाइल)",
        rankPrefix: "क्रमांक #",
        whyPrefix: "यह कार्ड पहले क्यों दिख रहा है? "
      },
      ta: {
        subhead: "இந்திய வானிலை ஆய்வு மையம் | புவி அறிவியல் அமைச்சகம்",
        searchPlaceholder: "மாவட்டம் / வானிலை மையம் தேடுங்கள்...",
        goBtn: "தேடு",
        mapTitle: "இந்திய வானிலை வரைபடம்",
        layerBase: "நிலையான வரைபடம்",
        layerRadar: "ரேடார் / மேகங்கள்",
        layerWarning: "எச்சரிக்கை பகுதிகள்",
        viewingMode: "முகப்புப் பக்க சுயவிவரம்",
        healthBtn: "சுகாதாரம் (AQI)",
        farmerBtn: "வேளாண் வானிலை",
        commuterBtn: "பயணிகள்",
        loading: "வானிலை தகவல்கள் பெறப்படுகின்றன...",
        topPriority: "முன்னுரிமை #1 (தேர்ந்தெடுக்கப்பட்டது)",
        rankPrefix: "வரிசை #",
        whyPrefix: "முன்னுரிமை விளக்கம்: "
      }
    };

    function initMap() {
      if (leafletMap) return;

      // Center strictly over India (Lat: 20.5937, Lon: 78.9629)
      leafletMap = L.map('india-map', {
        zoomControl: true,
        scrollWheelZoom: false
      }).setView([20.5937, 78.9629], 5);

      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; India Meteorological Department (IMD) | OpenStreetMap',
        maxZoom: 18,
        minZoom: 4
      }).addTo(leafletMap);
    }

    function setMapLayer(layerType) {
      document.querySelectorAll('.map-btn').forEach(btn => btn.classList.remove('active'));

      if (radarOverlayLayer && leafletMap.hasLayer(radarOverlayLayer)) {
        leafletMap.removeLayer(radarOverlayLayer);
      }

      if (layerType === 'base') {
        document.getElementById('btn-layer-base').classList.add('active');
      } else if (layerType === 'radar') {
        document.getElementById('btn-layer-radar').classList.add('active');
        // Real-time meteorological precipitation tile layer
        radarOverlayLayer = L.tileLayer('https://tile.openweathermap.org/map/precipitation_new/{z}/{x}/{y}.png?appid=439d4b804bc8187953eb36d2a8c26a02', {
          opacity: 0.65
        });
        radarOverlayLayer.addTo(leafletMap);
      } else if (layerType === 'warning') {
        document.getElementById('btn-layer-warning').classList.add('active');
        // Visual coastal alert circle simulation (e.g. Bay of Bengal Cyclone watch)
        radarOverlayLayer = L.circle([19.8135, 85.8312], {
          color: '#d90429',
          fillColor: '#ef4444',
          fillOpacity: 0.35,
          radius: 180000
        }).bindPopup("<b>IMD RED ALERT ZONE</b><br>Coastal squalls & heavy depression.");
        radarOverlayLayer.addTo(leafletMap);
      }
    }

    function updateMapMarker(cityName, stateName, temp) {
      if (!leafletMap) return;

      let coords = INDIAN_COORDS[cityName];
      if (!coords) {
        // Approximate fallback based on common geocoding or default pan
        coords = [20.5937, 78.9629];
      }

      leafletMap.flyTo(coords, 7, { animate: true, duration: 1.2 });

      if (currentMarker) {
        leafletMap.removeLayer(currentMarker);
      }

      currentMarker = L.marker(coords).addTo(leafletMap);
      currentMarker.bindPopup(`<b>${cityName} Station</b><br>
