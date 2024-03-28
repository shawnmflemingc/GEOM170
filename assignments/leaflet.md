# Leaflet Introduction

Leaflet can be an exciting journey into interactive web mapping. Leaflet is a widely-used, open-source JavaScript library that helps you create rich, interactive maps. It's known for its simplicity, performance, and usability, making it a perfect starting point for diving into JavaScript in this course.

## Setting Up Your First Map

1. **HTML File**: Start by creating an HTML file. This will be the backbone of your project. Here's a minimal example:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>My First Leaflet Map</title>
        <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
    </head>
    <body>
        <div id="map" style="height: 400px;"></div>
        <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
        <script>
            // JavaScript code to initialize the map will go here
        </script>
    </body>
    </html>
    ```

    In this HTML skeleton:
    - The Leaflet CSS file from the CDN unpkg.com is included in the `<head>` to style the map correctly. Note the SRI is provided to verify the package from the CDN (see https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity for more information)
    - Downloading and using the package is an option, but not a best practice (see https://leafletjs.com/download.html). 
    - A `<div>` element with an id of "map" is where our map will appear. We set a height style to ensure it's visible.
    - The Leaflet JavaScript library is included right before our script tag. This ensures it's loaded before we try to use it in our JavaScript code.

2. **Initializing the Map**: In the JavaScript section within the `<script>` tags, you can initialize your map and set its view to a specific geographical point and zoom level:

    ```javascript
    var map = L.map('map').setView([44.342, -78.741], 13);
    ```

    This code creates a new map in the 'map' div, centers it on the coordinates `[51.505, -0.09]` (latitude, longitude), and sets the zoom level to `13`.

3. **Adding a Tile Layer**: Maps are made up of tiles. You can add a tile layer to your map using a URL template where tiles are fetched from. OpenStreetMap is a common source for these tiles:

    ```javascript
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap contributors'
    }).addTo(map);
    ```

    This code adds the OpenStreetMap tiles to your map, providing a detailed and styled base map.

### Exploring Further

- **Markers**: You can add markers to your map to denote points of interest. For example, to add a marker at the same coordinates we centered our map on, you could use:

    ```javascript
    L.marker([44.342, -78.741]).addTo(map)
        .bindPopup('A pretty CSS3 popup at Fleming College.<br> Easily customizable.')
        .openPopup();
    ```

- **GeoJSON**: Leaflet has strong support for GeoJSON, a format for encoding a variety of geographic data structures. Adding GeoJSON data can allow you to represent more complex features like lines, shapes, and multi-part geometries.

```javascript
        // URL of the GeoJSON data
        var geojsonURL = 'https://shawnmflemingc.github.io/Geom170/datasets/2024eclipsepath.geojson';

        // Fetch the GeoJSON data
        fetch(geojsonURL)
            .then(function(response) {
                // Check if the request was successful
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                return response.json();  // Parse the response body as JSON
            })
            .then(function(data) {
                // Add the GeoJSON data to the map
                L.geoJSON(data).addTo(map);
            })
            .catch(function(error) {
                // Log any errors to the console
                console.error('Error fetching GeoJSON data:', error);
            });

```

- **Events**: Leaflet makes it easy to respond to user actions through event listeners. For instance, you might want to execute some code when the user clicks on the map.

### Resources

As you dive into Leaflet, here are a few resources that might be helpful:

- [Leaflet Quick Start Guide](https://leafletjs.com/examples/quick-start/): A great place to begin, with step-by-step instructions on creating a map.
- [Leaflet Tutorials](https://leafletjs.com/examples.html): More comprehensive guides covering various features of Leaflet.
- [Leaflet API Reference](https://leafletjs.com/reference.html): Detailed documentation on Leaflet's classes, methods, and options.

By starting with these basics, you'll be well on your way to creating interactive maps tailored to your needs. Remember, the best way to learn is by doing, so experiment with different Leaflet features and see what you can create!
