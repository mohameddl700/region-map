<!DOCTYPE html>
<html>
<head>
  <title>خريطة المدن حسب الـ Region</title>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  <style>
    #map { height: 600px; width: 100%; }
    .info-box { margin-top: 10px; font-size: 1.2em; }
  </style>
</head>
<body>
  <h2>خريطة المدن حسب الـ Region</h2>
  <input type="text" id="address" placeholder="اكتب العنوان هنا" style="width: 300px; padding: 5px;">
  <button onclick="suggestRegion()">اقتراح الـ Region</button>
  <div class="info-box" id="result"></div>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
  <script>
    // بيانات المدن والـRegions من ملف Excel بعد تحويله
    const cities = [
      { city: "10th of Ramdan City", region: "Al Sharqia" },
      { city: "6th of October", region: "Giza" },
      { city: "Abnoub", region: "Asyut" },
      { city: "Abo Sultan", region: "Ismailia" },
      { city: "Abo-Korkas", region: "Al Meniya" }
      // هنا هنضيف باقي المدن لاحقًا أو نحملهم من ملف JSON
    ];

    // إحداثيات تقريبية لكل مدينة - ممكن تتحدث لاحقًا
    const cityCoords = {
      "10th of Ramdan City": [30.306, 31.741],
      "6th of October": [29.976, 30.956],
      "Abnoub": [27.267, 31.151],
      "Abo Sultan": [30.200, 32.300],
      "Abo-Korkas": [27.930, 30.843]
    };

    const regionColors = {
      "Al Sharqia": "red",
      "Giza": "blue",
      "Asyut": "green",
      "Ismailia": "orange",
      "Al Meniya": "purple"
    };

    const map = L.map('map').setView([30.0, 31.0], 6);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 18,
    }).addTo(map);

    cities.forEach(({ city, region }) => {
      const coords = cityCoords[city];
      const color = regionColors[region] || 'gray';
      if (coords) {
        L.circleMarker(coords, {
          radius: 8,
          color,
          fillColor: color,
          fillOpacity: 0.7
        }).addTo(map).bindPopup(`<strong>${city}</strong><br>${region}`);
      }
    });

    function suggestRegion() {
      const input = document.getElementById('address').value.toLowerCase();
      const match = cities.find(({ city }) => input.includes(city.toLowerCase()));
      const resultBox = document.getElementById('result');
      if (match) {
        resultBox.innerHTML = `المدينة: <strong>${match.city}</strong><br>الـRegion: <strong>${match.region}</strong>`;
      } else {
        resultBox.innerHTML = "لم يتم العثور على تطابق. حاول كتابة اسم المدينة بشكل أوضح.";
      }
    }
  </script>
</body>
</html>
# region-map
