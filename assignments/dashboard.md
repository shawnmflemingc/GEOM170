# Leaflet and Chart.js Dashboard

Creating an interactive dashboard where a Leaflet map and a Chart.js chart are linked, such that the chart updates based on the map's view extent, involves several steps. This example assumes you have a basic understanding of both Leaflet and Chart.js, as well as how to handle GeoJSON data.

For this example, the map will display markers for each feature in the GeoJSON data, and the chart will update to show the count of features by month that are currently visible in the map's view. Here's a step-by-step guide:

### 1. Basic Setup

Start with the basic HTML structure, including loading the Leaflet and Chart.js libraries:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Map and Chart Dashboard</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        #map { height: 400px; }
        #chartContainer { width: 400px; height: 200px; }
    </style>
</head>
<body>

<div id="map"></div>
<div id="chartContainer">
    <canvas id="myChart"></canvas>
</div>

<script>
    // JavaScript code will go here
</script>

</body>
</html>
```

### 2. Initialize the Map

Inside the `<script>` tag, start by initializing your Leaflet map:

```javascript
var map = L.map('map').setView([51.505, -0.09], 13); // Adjust center and zoom level as needed

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
}).addTo(map);
```

### 3. Load and Display GeoJSON Data

Use Leaflet's `L.geoJSON` to add your GeoJSON data to the map. You'll need to fetch this data if it's hosted online:

```javascript
var geojsonLayer;

fetch('../datasets/ntp2023summary.geojson')
    .then(response => response.json())
    .then(data => {
        geojsonLayer = L.geoJSON(data, {
            onEachFeature: function(feature, layer) {
                layer.bindPopup(feature.properties.month); // Adjust based on your GeoJSON structure
            }
        }).addTo(map);

        updateChart(); // Initial chart update
    });
```

### 4. Initialize the Chart

Create a function to initialize the Chart.js chart. This function will also be used to update the chart data:

```javascript
var myChart;
var chartCtx = document.getElementById('myChart').getContext('2d');

function updateChart() {
    var visibleFeatures = [];
    geojsonLayer.eachLayer(function(layer) {
        if (map.getBounds().contains(layer.getLatLng())) {
            visibleFeatures.push(layer.feature);
        }
    });

    // Process visible features to get month counts
    var monthCounts = visibleFeatures.reduce(function(acc, feature) {
        var month = feature.properties.month; // Adjust based on your GeoJSON structure
        acc[month] = (acc[month] || 0) + 1;
        return acc;
    }, {});

    var chartLabels = Object.keys(monthCounts);
    var chartData = Object.values(monthCounts);

    // If chart exists, update data; else, create chart
    if (myChart) {
        myChart.data.labels = chartLabels;
        myChart.data.datasets[0].data = chartData;
        myChart.update();
    } else {
        myChart = new Chart(chartCtx, {
            type: 'bar',
            data: {
                labels: chartLabels,
                datasets: [{
                    label: 'Number of Records',
                    data: chartData,
                    backgroundColor: 'rgba(54, 162, 235, 0.2)',
                    borderColor: 'rgba(54, 162, 235, 1)',
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });
    }
}
```

### 5. Link Map View Changes to Chart Updates

Listen to the map's `moveend` event (which includes zoom changes and panning) to update the chart based on the current view:

```javascript
map.on('moveend', function() {
    updateChart();
});
```

This code structure sets up a basic linked view between a Leaflet map and a Chart.js chart, where the chart updates to reflect data (grouped by the `month` field) for GeoJSON features currently visible in the map's view. Adjust the GeoJSON structure and field names according to your specific dataset.
