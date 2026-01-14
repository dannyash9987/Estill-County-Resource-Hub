{% include nav.html %}

<html lang="en">
<head>
<meta charset="UTF-8">
<title>Estill County KY Weather Radar</title>

<!-- Leaflet CSS -->
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>

<style>
  body { margin: 0; padding: 0; }
  #map { height: 90vh; width: 100%; }
  #controls {
    height: 10vh;
    padding: 10px;
    background: #222;
    color: #fff;
    font-family: Arial, sans-serif;
  }
  input[type=range] {
    width: 100%;
  }
</style>
</head>

<body>

<div id="map"></div>

<div id="controls">
  <label>Radar Time:</label>
  <input type="range" id="timeSlider" min="0" max="0" value="0">
  <div id="timeLabel"></div>
</div>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
const map = L.map('map').setView([37.68, -83.97], 10);

// Base map
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  maxZoom: 18
}).addTo(map);

// City markers
L.marker([37.70, -83.97]).addTo(map)
  .bindPopup("<b>Irvine, KY</b>");

L.marker([37.73, -84.03]).addTo(map)
  .bindPopup("<b>Ravenna, KY</b>");

// RainViewer radar setup
let radarLayer = null;
let radarFrames = [];
let currentFrame = 0;

// Fetch radar data
fetch("https://api.rainviewer.com/public/weather-maps.json")
  .then(res => res.json())
  .then(data => {
    radarFrames = [
      ...data.radar.past,
      ...data.radar.nowcast   // near-future frames
    ];

    const slider = document.getElementById("timeSlider");
    slider.max = radarFrames.length - 1;

    updateRadar(0);

    slider.addEventListener("input", e => {
      updateRadar(e.target.value);
    });
  });

function updateRadar(index) {
  const frame = radarFrames[index];

  if (radarLayer) {
    map.removeLayer(radarLayer);
  }

  radarLayer = L.tileLayer(
    `https://tilecache.rainviewer.com/v2/radar/${frame.path}/256/{z}/{x}/{y}/2/1_1.png`,
    { opacity: 0.7 }
  ).addTo(map);

  document.getElementById("timeLabel").innerText =
    new Date(frame.time * 1000).toLocaleString();
}
</script>

</body>
</html>
