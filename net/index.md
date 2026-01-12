{% include nav.html %}

<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Estill County, Kentucky Map</title>

    <!-- Leaflet CSS -->
    <link
        rel="stylesheet"
        href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    />

    <style>
        #map {
            height: 600px;
            width: 100%;
        }

        .town-label {
            background: rgba(255, 255, 255, 0.85);
            border: none;
            box-shadow: none;
            font-size: 13px;
            font-weight: bold;
            color: #333;
            padding: 2px 6px;
        }

        .town-label.large {
            font-size: 15px;
        }
    </style>
</head>
<body>

<h2>Interactive Map of Estill County, Kentucky</h2>
<div id="map"></div>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
    // Create the map
    const map = L.map('map').setView([37.697, -83.968], 10);

    // OpenStreetMap tiles
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

    // Updated town data (corrected)
    const towns = [
        {
            name: "Irvine",
            coords: [37.7006, -83.9732],
            description: "County Seat of Estill County",
            large: true
        },
        {
            name: "Ravenna",
            // Corrected coordinates
            coords: [37.6845, -83.9530],
            description: "City in Estill County",
            large: false
        }
    ];

    // Add markers and labels
    towns.forEach(town => {
        const marker = L.circleMarker(town.coords, {
            radius: 5,
            fillColor: "#2c7fb8",
            color: "#08519c",
            weight: 1,
            fillOpacity: 0.9
        }).addTo(map);

        marker.bindPopup(`<b>${town.name}</b><br>${town.description}`);

        marker.bindTooltip(town.name, {
            permanent: true,
            direction: "right",
            className: town.large ? "town-label large" : "town-label",
            offset: [8, 0]
        });
    });
</script>

</body>
</html>
