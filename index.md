---
layout: default
title: Home
hero: true
---

{% include nav.html %}

Welcome! This is the Resource Hub for **Estill County KY**!

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Estill County Weather Forecast</title>
</head>
<body>
    <h2>Weather Forecast — Estill County, KY</h2>
    <div id="forecast"></div>

<script>
    // Replace with your own API key from Visual Crossing
    const API_KEY = "YOUR_API_KEY_HERE";

    // Estill County Coordinates
    const lat = 37.697;
    const lon = -83.968;

    async function getWeatherForecast() {
        const url = 
            `https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/` +
            `${lat},${lon}/next7days?unit=us&key=${API_KEY}`;

        try {
            const response = await fetch(url);
            if (!response.ok) throw new Error("Network response was not OK");

            const data = await response.json();

            displayForecast(data);
        } catch (err) {
            document.getElementById("forecast").innerText =
                "Error fetching weather data: " + err.message;
        }
    }

    function displayForecast(data) {
        const container = document.getElementById("forecast");
        container.innerHTML = "";

        if (data.days && data.days.length > 0) {
            data.days.forEach(day => {
                const dayEl = document.createElement("div");
                dayEl.style.border = "1px solid #ccc";
                dayEl.style.padding = "10px";
                dayEl.style.margin = "8px 0";

                dayEl.innerHTML = `
                    <strong>${day.datetime}</strong><br>
                    Conditions: ${day.conditions}<br>
                    High: ${day.tempmax}°F, Low: ${day.tempmin}°F<br>
                    Precipitation: ${day.precipprob}%<br>
                `;

                container.appendChild(dayEl);
            });
        } else {
            container.innerText = "No forecast data found.";
        }
    }

    // Run fetch on load
    getWeatherForecast();
</script>

</body>
</html>

Navigate with the links below:

- [Home](/Estill-County-Resource-Hub/)
- [Where can Local Gov. be found?](/Estill-County-Resource-Hub/kb/)
- [Local Government Off. #'s](/Estill-County-Resource-Hub/logs/)
- [Map of County](/Estill-County-Resource-Hub/net/)
