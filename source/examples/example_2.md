---
layout: post
title: Display tool tips
---

# Display tool tips

Display tool tips with the name and description of verified points. This example uses `Deck.gl` and `Mapbox tiles`.

### Demo

{% iframe ../demos/example_2_demo.html 100% 450 %}

### See also

+ [Deck.gl Developer Guide](https://deck.gl/#/documentation/developer-guide/adding-interactivity?section=example-display-a-tooltip-for-hovered-object)
+ [Mapbox access token](https://www.mapbox.com/)
+ [Bounding Box Helper](https://boundingbox.klokantech.com/)
+ [FOAM API - Swagger UI](../swagger/ui.html)

### Source

```html
<html>
  <head>
    <title>FOAM Map API Example</title>
    <script src="https://unpkg.com/deck.gl@^8.0.0/dist.min.js"></script>
    <script src="https://unpkg.com/latlon-geohash@^1.1.0/latlon-geohash.js"></script>
    <script src="https://api.tiles.mapbox.com/mapbox-gl-js/v1.7.0/mapbox-gl.js"></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v1.7.0/mapbox-gl.css' rel='stylesheet' />
    <style type="text/css">
      body {
        width: 100vw;
        height: 100vh;
        margin: 0;
      }
      
      #tooltip {
        position: absolute;
        z-index: 1;
        pointer-events: none;
        color: #FFFFFF;
        padding: 0 16px;
        max-width: 320px;
        border-radius: 12px;
        background: rgba(38, 171, 95, 0.9)
      }
    </style>
  </head>

  <body>
    <canvas id="deck-canvas"></canvas>
    <div id="tooltip"></div>
  </body>

  <script type="text/javascript">

    const {DeckGL, ScatterplotLayer} = deck;

	  // Colors
    const FOAM_PENDING_COLOR = [46, 124, 230];
    const FOAM_VERIFIED_COLOR = [38, 171, 95];
    const FOAM_CHALLENGED_COLOR = [244, 128, 104];
    const FOAM_REMOVED_COLOR = [255, 0, 0];
    
    // Map layout
    const BOUNDING_BOX = [
      [-73.9645, 40.6885],
      [-73.9845, 40.7085]
    ];
    const INITIAL_ZOOM = 16;
    const MAX_ZOOM = 16;
    const RADIUS_SCALE = 50;
    const RADIUS_MIN_PIXELS = 1;
    const RADIUS_MAX_PIXELS = 10;
    
    // Tooltip
    var listingHashDisplayed;
    var fetchingListingHash = false;
    
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
    
    // For hovered point,
    // Load data from external source
    // Inject data in tooltip
    function updateTooltipDescription(listingHash) {
      if (!fetchingListingHash && listingHashDisplayed !== listingHash) {
        fetchingListingHash = true
        fetch('https://map-api-direct.foam.space/poi/' + listingHash)
          .then(response => response.json())
          .then((json) => {
            if (json && json.data && json.data.data) {
              const el = document.getElementById('tooltip');
              const tooltip = `<h3>${json.data.data.name}</h3><p>${json.data.data.description}</p>`
              el.innerHTML = tooltip;
              listingHashDisplayed = listingHash
              fetchingListingHash = false
            }
        });
      }
    }
    
    // Load data from external source
    // Use data in a new DeckGL object
    const DATA_URL = 'https://map-api-direct.foam.space/poi/filtered?swLng=' + BOUNDING_BOX[0][0] + '&swLat=' + BOUNDING_BOX[0][1] + '&neLng=' + BOUNDING_BOX[1][0] + '&neLat=' + BOUNDING_BOX[1][1] + '&status=listing&sort=most_value&limit=100&offset=0'
    fetch(DATA_URL)
      .then(response => response.json())
      .then(data =>
        new DeckGL({
          mapboxApiAccessToken: '<mapbox-access-token>', // Replace with your Mapbox access token
          mapStyle: 'mapbox://styles/mapbox/dark-v10',
          initialViewState: {
            longitude: getCenterPoint(BOUNDING_BOX)[0],
            latitude: getCenterPoint(BOUNDING_BOX)[1],
            zoom: INITIAL_ZOOM,
            maxZoom: MAX_ZOOM,
          },
          layers: [
            new ScatterplotLayer({
              id: 'scatter-plot',
              data,
              radiusScale: RADIUS_SCALE,
              radiusMinPixels: RADIUS_MIN_PIXELS,
              radiusMaxPixels: RADIUS_MAX_PIXELS,
              getPosition: d => getPointCoords(d['geohash']),
              getFillColor: d => getPointColor(d.state),
              pickable: true,
              onHover: ({object, x, y}) => {
                const el = document.getElementById('tooltip');
                if (object) {
                  updateTooltipDescription(object.listingHash);
                  el.style.display = 'block';
                  el.style.left = x + 'px';
                  el.style.top = y + 'px';
                } else {
                  el.style.display = 'none'; 
                  el.innerHTML = '';
                  listingHashDisplayed = '';
                }
              }
            })
          ]
        })
      );

  </script>
</html>
```