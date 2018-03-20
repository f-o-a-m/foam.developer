---
title: Swagger UI
---

# Authentication

We use your Ethereum address for authentication. For detailed information how to authenticate with our API, see [auth](../tutorials/intro_to_api.html).

Grab your bearer token from [SpatialIndex](https://beta.foam.space) and paste "Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" into the `Authorize` button below.

Click "default" below to explore the API, or load the definition into Postman or similar programs.

# Swagger UI

{% swagger_ui_advanced ./swagger.json  %}
{
  "version": 3
}
{% endswagger_ui_advanced %}
