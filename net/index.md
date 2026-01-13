{% include nav.html %}

<html>
<head>
    <meta charset="UTF-8">
    <title>Estill County Radar Map</title>

    <!-- Leaflet CSS -->
    <link
      rel="stylesheet"
      href="https://unpkg.com/leaflet/dist/leaflet.css"
    />

    <style>
      #map { height: 90vh; }
      #sliderContainer {
        text-align: center;
        margin-top: 4px;
      }
    </style>
</head>
<body>

<div id="map"></div>
<div id="sliderContainer">
    <input type="range" id="timeSlider" min="0" max="0" step="1" />
    <span id="timeLabel">Loading...</span>
</div>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

<script>
(async function () {
    // 1) Create map
    const map = L.map("map").setView([37.7, -83.96], 11);

    // Base layer
    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution: "© OpenStreetMap contributors",
    }).addTo(map);

    // 2) Add markers for Irvine & Ravenna
    const irvineMarker = L.marker([37.7006, -83.9738]).addTo(map)
        .bindPopup("Irvine, KY");
    const ravennaMarker = L.marker([37.6845, -83.9530]).addTo(map)
        .bindPopup("Ravenna, KY");

    // 3) Load radar times from RainViewer API
    const radarJson = await fetch("https://api.rainviewer.com/public/weather-maps.json")
        .then(r => r.json());

    const radarFrames = radarJson.radar.past.concat(radarJson.radar.nowcast || []);
    if (!radarFrames || !radarFrames.length) {
        document.getElementById("timeLabel").textContent = "No Radar Data";
        return;
    }

    // 4) Create radar layer container
    let radarLayer = null;

    // 5) Slider setup
    const slider = document.getElementById("timeSlider");
    slider.max = radarFrames.length - 1;
    slider.oninput = () => showFrame(slider.value);

    function showFrame(index) {
        const frame = radarFrames[index];

        document.getElementById("timeLabel").textContent =
            new Date(frame.time * 1000).toLocaleString();

        const urlTemplate =
            radarJson.host +
            frame.path +
            "/{z}/{x}/{y}/2/1_1.png";

        if (radarLayer) map.removeLayer(radarLayer);

        radarLayer = L.tileLayer(urlTemplate, {
          opacity: 0.5,
          pane: "overlayPane"
        }).addTo(map);
    }

    // Show first frame
    showFrame(0);

})();
</script>

</body>
</html>

