{% include nav.html %}

<html lang="en">
<head>
<meta charset="UTF-8">
<title>Estill County KY Weather Map</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

<style>
#map {
    height: 700px;
    width: 100%;
}

.town-label {
    background: rgba(255,255,255,0.85);
    border: none;
    font-weight: bold;
    font-size: 13px;
    padding: 2px 6px;
}

.alert-box {
    position: absolute;
    bottom: 10px;
    left: 10px;
    background: white;
    padding: 10px;
    max-width: 300px;
    font-size: 13px;
    z-index: 999;
    border-left: 6px solid red;
}
</style>
</head>

<body>

<h2>Estill County, Kentucky — Live Weather Map</h2>
<div id="map"></div>
<div id="alerts" class="alert-box">Loading alerts…</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
// =======================
// 1. MAP SETUP
// =======================
const map = L.map("map").setView([37.697, -83.98], 11);

const osm = L.tileLayer(
  "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  { attribution: "&copy; OpenStreetMap" }
).addTo(map);

// =======================
// 2. CITIES
// =======================
[
  { name: "Irvine", coords: [37.7006, -83.9732] },
  { name: "Ravenna", coords: [37.6948, -84.0280] }
].forEach(town => {
  L.circleMarker(town.coords, {
    radius: 6,
    color: "#08519c",
    fillColor: "#2c7fb8",
    fillOpacity: 0.9
  }).addTo(map)
    .bindTooltip(town.name, { permanent: true, className: "town-label" });
});

// =======================
// 3. ESTILL COUNTY BOUNDARY
// =======================
// Simplified GeoJSON boundary
fetch("https://raw.githubusercontent.com/plotly/datasets/master/geojson-counties-fips.json")
  .then(r => r.json())
  .then(data => {
    const estill = data.features.find(f => f.id === "21065");
    L.geoJSON(estill, {
      style: {
        color: "#000",
        weight: 2,
        fillOpacity: 0.05
      }
    }).addTo(map);
  });

// =======================
// 4. RADAR ANIMATION (RainViewer)
// =======================
let radarLayers = [];
let radarIndex = 0;

fetch("https://api.rainviewer.com/public/weather-maps.json")
  .then(r => r.json())
  .then(data => {
    data.radar.past.slice(-6).forEach(frame => {
      radarLayers.push(
        L.tileLayer(
          `https://tilecache.rainviewer.com/v2/radar/${frame.time}/256/{z}/{x}/{y}/2/1_1.png`,
          { opacity: 0.6 }
        )
      );
    });

    radarLayers[0].addTo(map);

    setInterval(() => {
      map.removeLayer(radarLayers[radarIndex]);
      radarIndex = (radarIndex + 1) % radarLayers.length;
      radarLayers[radarIndex].addTo(map);
    }, 1200);
  });

// =======================
// 5. STORM CELL TRACKING
// =======================
const stormCells = L.tileLayer(
  "https://tilecache.rainviewer.com/v2/nowcast/{z}/{x}/{y}/2/1_1.png",
  { opacity: 0.5 }
);

// =======================
// 6. NWS ALERTS (NO API KEY)
// =======================
fetch("https://api.weather.gov/alerts/active?area=KY")
  .then(r => r.json())
  .then(data => {
    const estillAlerts = data.features.filter(alert =>
      alert.properties.areaDesc.includes("Estill")
    );

    const alertDiv = document.getElementById("alerts");

    if (estillAlerts.length === 0) {
      alertDiv.innerHTML = "<b>No active alerts for Estill County</b>";
      alertDiv.style.borderLeft = "6px solid green";
    } else {
      alertDiv.innerHTML = estillAlerts.map(a =>
        `<b>${a.properties.event}</b><br>${a.properties.headline}`
      ).join("<hr>");
    }
  });

// =======================
// 7. LAYER CONTROL
// =======================
L.control.layers(
  { "OpenStreetMap": osm },
  {
    "Storm Cell Tracking": stormCells
  },
  { collapsed: false }
).addTo(map);

</script>
</body>
</html>
