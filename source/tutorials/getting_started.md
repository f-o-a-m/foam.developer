title: Getting started
---

# Getting started with the FOAM API

This document provides details on how to make authenticated requests to the FOAM REST API to obtain data to produce various web maps shown below.

Thank you [Will Carter](https://github.com/FergusDevelopmentLLC/) for writing these examples!

## Metamask and Rinkeby

You will need [Metamask](https://metamask.io/) installed in your Chrome browser.

Next, you will need to use the [Rinkeby Faucet](https://www.rinkeby.io/#faucet) to add funds to your Rinkeby wallet.

## Obtain authentication for the FOAM REST API

The FOAM REST API requires a header authorization key/value pair for each request. 

Follow the [this tutorial](intro_to_api.html) to obtain the Bearer auth string value:

The Bearer string will be similar to:
```
Bearer fxtguifkduUxMiJ9.eyJkYXQdrceveM5NTFmMhSc8mHjvwph9eUOLX8vGgt6cbkqdlrzqtvw...
```

## Send a test authenticated API request with Postman

1. Install [Postman](https://www.getpostman.com/) a REST API utility 
2. Download the `swagger.json` definition of the FOAM REST API. This definition file can then be imported into Postman, which will provide easy access ot all the FOAM API endpoints. Download [here](https://f-o-a-m.github.io/foam.developer/3f1f223f5e0965a733c42bdba28adbf5/swagger.json) - if the link doesn't work go to the [swagger link](../swagger/ui.html) to the left and get the latest link.
3. Import `swagger.json` into Postman to gain access to all the FOAM API endpoints (see screengrab below).
4. Double click the `https://api-beta.foam.space/beacon?lat_min={{lat_min}}&lon_min={{lon_min}}&lat_max={{lat_max}}&lon_max={{lon_max}}&zoom={{zoom}}` API method and replace the GET url with one that contains actual bounding box coordinates, for example: 
```
https://api-beta.foam.space/beacon?lat_min=-74.024677&lon_min=-73.923054&lat_max=40.695998&lon_max=40.802245
```
5. Add an authorization key with the Bearer auth string from the Authorization step above (see screengrab below).
6. Send test request.

![](https://i.imgur.com/w3E0UoA.gif)

## Find a bounding box

[bboxfinder](http://bboxfinder.com) is a useful utility for finding a Latitude/Longitude bounding box that matches your geographic area of interest. 

![](http://storage5.static.itmages.com/i/18/0323/h_1521817819_9414564_3c23fc1852.png)

Note, when sending a GET /beacon request to the FOAM API, use the latitude/longitude values of your area of interest's bounding box.
* `lat_min` = smallest latitude value
* `lat_max` = largest latitude value
* `lon_min` = smallest longitude value
* `lon_max` = largest longitude value

## Example Maps

### Leaflet

This is an example of a simple Leaflet.js map at displays FOAM CSC data by calling the API. The example uses the [latlon-geohash](https://github.com/chrisveness/latlon-geohash) library for geohash to latitude/longitude datapoint conversion and [axios](https://github.com/axios/axios) for making REST calls to the FOAM API.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?lat_min={{lat_min}}&lon_min={{lon_min}}&lat_max={{lat_max}}&lon_max={{lon_max}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?lat_min=-105.154953&lon_min=39.580819&lat_max=-104.646835&lon_max=39.941857
```

![](http://storage3.static.itmages.com/i/18/0322/h_1521738128_2846268_36122b1f75.png)

* [Live example](http://bl.ocks.org/kejace/0d93e33c0da9696b0fc5db3b0fbae06d)
* Leaflet [API docs](http://leafletjs.com/reference-1.3.0.html)

There are many different Leaflet [basemaps](http://leaflet-extras.github.io/leaflet-providers/preview/) available.

### Mapbox.GL

This is an example of a simple Mapbox.GL map at displays FOAM CSC data by calling the API. The example uses the [latlon-geohash](https://github.com/chrisveness/latlon-geohash) library for geohash to latitude/longitude datapoint conversion and [axios](https://github.com/axios/axios) for making REST calls to the FOAM API.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?lat_min={{lat_min}}&lon_min={{lon_min}}&lat_max={{lat_max}}&lon_max={{lon_max}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?lat_min=-105.244904&lon_min=-104.648895&lat_max=39.515695&lon_max=39.985538
```

![](http://storage7.static.itmages.com/i/18/0322/h_1521738283_6444740_02c3e5b2d8.png)

* [Live example](http://bl.ocks.org/kejace/c312c8ba8f05c910b6f09dcc81212bd8)
* Mapbox.GL [API docs](https://www.mapbox.com/mapbox-gl-js/api/)

### Custom markers

Similar to the example above but with a different base map and custom CSC markers.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?lat_min={{lat_min}}&lon_min={{lon_min}}&lat_max={{lat_max}}&lon_max={{lon_max}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?lat_min=-11.030273&lon_min=27.202148&lat_max=35.38905&lon_max=58.950008
```

![](http://storage8.static.itmages.com/i/18/0322/h_1521738397_6180381_d2d1d3856e.png)

* [Live example](http://bl.ocks.org/kejace/42614058a148d40c549ec886dcf7bdbd)
* Mapbox.GL [custom markers](https://www.mapbox.com/help/custom-markers-gl-js/)

### Data popup on hover

This example illustrates how to add a marker popup on mouse hover of a point.

FOAM REST API request:
```
GET /beacon
https://api-beta.foam.space/beacon?lat_min={{lat_min}}&lon_min={{lon_min}}&lat_max={{lat_max}}&lon_max={{lon_max}}&zoom={{zoom}}

Example request:
https://api-beta.foam.space/beacon?lat_min=-74.024677&lon_min=-73.923054&lat_max=40.695998&lon_max=40.802245
```

![](http://storage6.static.itmages.com/i/18/0322/h_1521738706_3252718_a826d36491.png)

* [Live example](http://bl.ocks.org/kejace/356a4f31773a2edc9b1b1fec676bdfaf)
* [Display a popup](https://www.mapbox.com/mapbox-gl-js/example/popup/)

### CSC search

Example CSC search implementation. Searches that will find matched names: "Denver2", "London", "ParisFoamBeta1", "Marriott hotel" or any other beacon name.

FOAM REST API requests:
```
GET /search
https://api-beta.foam.space/search?offset={{offset}}&query={{query}}
https://api-beta.foam.space/search?query=London123

then:

GET /beacon/{address}
https://api-beta.foam.space/beacon/:address
https://api-beta.foam.space/beacon/4ba12ad784ff4e596175cd3f38d04d582030561b

```
![](https://i.imgur.com/hqGX8qx.gif)

* [Live example](http://bl.ocks.org/kejace/e29a2744e533131adfec393eb18ed1f3)
