<!DOCTYPE html>
<html>
<head>
  <title>Live Location on Map</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link
    rel="stylesheet"
    href="https://unpkg.com/leaflet@1.9.3/dist/leaflet.css" />
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      text-align: center;
    }

    h2 {
      margin: 20px 0;
    }

    #map {
      height: 90vh;
      width: 100%;
    }

    #status {
      margin: 10px;
      font-weight: bold;
      color: #444;
    }
  </style>
</head>
<body>

<h2>🌍 My Live Location</h2>
<div id="status">Getting your location...</div>
<div id="map"></div>

<script src="https://unpkg.com/leaflet@1.9.3/dist/leaflet.js"></script>
<script>
  const map = L.map('map').setView([0, 0], 2); // Default world view

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);

  function showLocation(lat, lon) {
    map.setView([lat, lon], 13);
    L.marker([lat, lon]).addTo(map)
      .bindPopup("You are here!")
      .openPopup();
    document.getElementById("status").textContent = `Latitude: ${lat.toFixed(6)} | Longitude: ${lon.toFixed(6)}`;
  }

  function getLocation() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (pos) => {
          const lat = pos.coords.latitude;
          const lon = pos.coords.longitude;
          showLocation(lat, lon);
        },
        (err) => {
          document.getElementById("status").textContent = "❌ Location Error: " + err.message;
        }
      );
    } else {
      document.getElementById("status").textContent = "❌ Geolocation not supported.";
    }
  }

  window.onload = getLocation;
</script>

</body>
</html>
