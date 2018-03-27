---
title: Swagger UI
---

# Introduction

The following page describes the FOAM API using a [swagger](https://swagger.io/) definition.

## Authentication

We use your Ethereum address for authentication. For detailed information how to authenticate with our API, see our [tutorial](../tutorials/intro_to_api.html).

Grab your bearer token from [SpatialIndex](https://beta.foam.space) and paste "Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" into the `Authorize` button below.

Click "foam" below to explore the API. You can also expand the "Models" to explore the model defined by our API.
A simple way to interact with the API is to load the definition into [Postman](https://www.getpostman.com/) or [Insomnia](https://insomnia.rest/) for example.

## Warning

The following describes a _beta_ of the FOAM API. **Schema changes can and likely will occur at any time in the future.**

## Swagger UI

{% swagger_ui_advanced https://api-beta.foam.space/swagger.json %}
{
  "version": 3,
  "doc_expansion": "none"
}
{% endswagger_ui_advanced %}
