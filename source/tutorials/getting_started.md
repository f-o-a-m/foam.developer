title: Getting started
---

# Getting started with the FOAM API

This document provides details on how to make authenticated requests to the FOAM REST API to obtain data to produce various web maps shown below.

## Obtain authentication for the FOAM REST API

The FOAM REST API requires a header authorization key/value pair for each request. Follow the [this tutorial](intro_to_api.html) to obtain the Bearer auth string value:

The Bearer string will be similar to:
```
Bearer fxtguifkduUxMiJ9.eyJkYXQdrceveM5NTFmMhSc8mHjvwph9eUOLX8vGgt6cbkqdlrzqtvw...
```

## Send a test authenticated API request with Postman

1. Install [Postman](https://www.getpostman.com/) a REST API utility 
2. Download the `swagger.json` definition of the FOAM REST API. This definition file can then be imported into Postman, which will provide easy access ot all the FOAM API endpoints. Download [here](https://f-o-a-m.github.io/foam.developer/3f1f223f5e0965a733c42bdba28adbf5/swagger.json) - if the link doesn't work go to the [swagger link](../swagger/ui.html) to the left and get the latest link.
3. Import `swagger.json` into Postman to gain access to all the FOAM API endpoints (see screengrab below).
4. Double click the `https://api-beta.foam.space/beacon?xmin={{xmin}}&ymin={{ymin}}&xmax={{xmax}}&ymax={{ymax}}&zoom={{zoom}}` API method and replace the GET url with one that contains actual bounding box coordinates, for example: 
```
https://api-beta.foam.space/beacon?xmin=-74.024677&ymin=-73.923054&xmax=40.695998&ymax=40.802245
```
5. Add an authorization key with the Bearer auth string from the Authorization step above (see screengrab below).
6. Send test request.

![](https://i.imgur.com/w3E0UoA.gif)

## Find a bounding box

http://bboxfinder.com is a useful utility for finding a Latitude/Longitude bounding box that matches your geographic area of interest. 

![](http://storage5.static.itmages.com/i/18/0323/h_1521817819_9414564_3c23fc1852.png)

Note, when sending a GET /beacon request to the FOAM API, use the latitude/longitude values of your area of interest's bounding box.
* `xmin` = smallest longitude value
* `xmax` = largest longitude value
* `ymin` = smallest latitude value
* `ymax` = largest latitude value

## Example Maps

See code examples [here](https://github.com/FergusDevelopmentLLC/foam-api-examples/tree/master/examples).

### Leaflet

This is an example of a simple Leaflet.js map at displays FOAM CSC data by calling the API. The example uses the [latlon-geohash](https://github.com/chrisveness/latlon-geohash) library for geohash to latitude/longitude datapoint conversion and [axios](https://github.com/axios/axios) for making REST calls to the FOAM API.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?xmin={{xmin}}&ymin={{ymin}}&xmax={{xmax}}&ymax={{ymax}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?xmin=-105.154953&ymin=39.580819&xmax=-104.646835&ymax=39.941857
```

![](http://storage3.static.itmages.com/i/18/0322/h_1521738128_2846268_36122b1f75.png)

* [Live example](http://bl.ocks.org/FergusDevelopmentLLC/3b3fd8491b3df85e40d6e0d4b9911493)
* Leaflet [API docs](http://leafletjs.com/reference-1.3.0.html)

There are many different Leaflet [basemaps](http://leaflet-extras.github.io/leaflet-providers/preview/) available.

### Mapbox.GL

This is an example of a simple Mapbox.GL map at displays FOAM CSC data by calling the API. The example uses the [latlon-geohash](https://github.com/chrisveness/latlon-geohash) library for geohash to latitude/longitude datapoint conversion and [axios](https://github.com/axios/axios) for making REST calls to the FOAM API.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?xmin={{xmin}}&ymin={{ymin}}&xmax={{xmax}}&ymax={{ymax}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?xmin=-105.244904&ymin=-104.648895&xmax=39.515695&ymax=39.985538
```

![](http://storage7.static.itmages.com/i/18/0322/h_1521738283_6444740_02c3e5b2d8.png)

* [Live example](http://bl.ocks.org/FergusDevelopmentLLC/e1cb1d18dac41c46c72d8c19f7ef09c8)
* Mapbox.GL [API docs](https://www.mapbox.com/mapbox-gl-js/api/)

### Custom markers

Similar to the example above but with a different base map and custom CSC markers.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?xmin={{xmin}}&ymin={{ymin}}&xmax={{xmax}}&ymax={{ymax}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?xmin=-11.030273&ymin=27.202148&xmax=35.38905&ymax=58.950008
```

![](http://storage8.static.itmages.com/i/18/0322/h_1521738397_6180381_d2d1d3856e.png)

* [Live example](http://bl.ocks.org/FergusDevelopmentLLC/5769c878d00d8f67569ae5b52c83caad)
* Mapbox.GL [custom markers](https://www.mapbox.com/help/custom-markers-gl-js/)

### Data popup on hover

This example illustrates how to add a marker popup on mouse hover of a point.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?xmin={{xmin}}&ymin={{ymin}}&xmax={{xmax}}&ymax={{ymax}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?xmin=-74.024677&ymin=-73.923054&xmax=40.695998&ymax=40.802245
```

![](http://storage6.static.itmages.com/i/18/0322/h_1521738706_3252718_a826d36491.png)

* [Live example](http://bl.ocks.org/FergusDevelopmentLLC/020933cd26b2133291029fce53a457fb)
* [Display a popup](https://www.mapbox.com/mapbox-gl-js/example/popup/)

### CSC search

Example CSC search implementation. Searches that will find matched names: "Denver2", "London", "ParisFoamBeta1", "Marriott hotel" or any other beacon name.

FOAM REST API requests:
```
GET /search
https://api-beta.foam.space/search?offset={{offset}}&q={{q}}
https://api-beta.foam.space/search?q=London123

then:

GET /beacon/{address}
https://api-beta.foam.space/beacon/:address
https://api-beta.foam.space/beacon/4ba12ad784ff4e596175cd3f38d04d582030561b

```
![](https://i.imgur.com/hqGX8qx.gif)

* [Live example](http://bl.ocks.org/FergusDevelopmentLLC/70150641ddb8c7eb93cebcc689faaae8)
