---
layout: post
title: Plot points
---

# Plot points

Display a map with color-coded points. This example uses `Deck.gl` and `Mapbox tiles`.

### Demo

{% iframe ../demos/example_1_demo.html 100% 450 %}

### See also

+ [Deck.gl API Reference](https://deck.gl/#/documentation/deckgl-api-reference/layers/scatterplot-layer)
+ [Mapbox access token](https://www.mapbox.com/)
+ [Bounding Box Helper](https://boundingbox.klokantech.com/)
+ [FOAM API - Swagger UI](../swagger/ui.html)

### Source

```html
<html>
  <head>
    <title>FOAM Map API Example</title>
    <script src="https://unpkg.com/deck.gl@^7.0.0/dist.min.js"></script>
    <script src="https://unpkg.com/latlon-geohash@^1.1.0/latlon-geohash.js"></script>
    <script src="https://api.tiles.mapbox.com/mapbox-gl-js/v0.50.0/mapbox-gl.js"></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v1.0.0/mapbox-gl.css' rel='stylesheet' />
    <style type="text/css">
      body {
        width: 100vw;
        height: 100vh;
        margin: 0;
      }
    </style>
  </head>
  <body></body>

  <script type="text/javascript">

    const {DeckGL, ScatterplotLayer} = deck;

	// Colors
    const FOAM_PENDING_COLOR = [46, 124, 230];
    const FOAM_VERIFIED_COLOR = [38, 171, 95];
    const FOAM_CHALLENGED_COLOR = [244, 128, 104];
    const FOAM_REMOVED_COLOR = [255, 0, 0];
    
    // Map layout
    const BOUNDING_BOX = [
      [-74.0145, 40.6585],
      [-73.9345, 40.7485]
    ];
    const INITIAL_ZOOM = 12.5;
    const MAX_ZOOM = 16;
    const RADIUS_SCALE = 50;
    const RADIUS_MIN_PIXELS = 1;
    const RADIUS_MAX_PIXELS = 2.5;
    
    // Functions
    function getCenterPoint(bounding_box) {
      return [(bounding_box[0][0] + bounding_box[1][0]) / 2, (bounding_box[0][1] + bounding_box[1][1]) / 2];
    }
    
    function getPointColor(state) {
        if (state && state.status && state.status.type) {
          if (state.status.type === "applied") { return FOAM_PENDING_COLOR }
          else if (state.status.type === "listing") { return FOAM_VERIFIED_COLOR }
          else if (state.status.type === "challenged") { return FOAM_CHALLENGED_COLOR }
        } else {
          return FOAM_REMOVED_COLOR
        }
    }
    
    function getPointCoords(geohash) {
    	coords = Geohash.decode(geohash);
    	return [coords['lon'], coords['lat'], 0];
    }
    
    // DeckGL
    new DeckGL({
      mapboxApiAccessToken: '<mapbox-access-token>', // Replace with your Mapbox access token
      mapStyle: 'mapbox://styles/mapbox/dark-v10',
      longitude: getCenterPoint(BOUNDING_BOX)[0],
      latitude: getCenterPoint(BOUNDING_BOX)[1],
      zoom: INITIAL_ZOOM,
      maxZoom: MAX_ZOOM,
      layers: [
        new ScatterplotLayer({
          id: 'scatter-plot',
          data: 'https://map-api-direct.foam.space/poi/filtered?swLng=' + BOUNDING_BOX[0][0] + '&swLat=' + BOUNDING_BOX[0][1] + '&neLng=' + BOUNDING_BOX[1][0] + '&neLat=' + BOUNDING_BOX[1][1] + '&status=application&status=listing&status=challenged&status=removed&sort=most_value&limit=500&offset=0',
          radiusScale: RADIUS_SCALE,
          radiusMinPixels: RADIUS_MIN_PIXELS,
          radiusMaxPixels: RADIUS_MAX_PIXELS,
          getPosition: d => getPointCoords(d['geohash']),
          getFillColor: d => getPointColor(d.state)
        })
      ]
    });

  </script>
</html>
```