---
title: Clarifai
description: Instructions on how to integrate Clarifai with Home Assistant.
ha_category:
  - Image Processing
ha_release: 0.112
ha_domain: clarifai
ha_codeowners:
  - '@djbowers'
---

The `clarifai` integration allows you to use the
[Clarifai](https://www.clarifai.com/) API through Home Assistant.
To get started, create a free account on the Clarifai [Portal](https://portal.clarifai.com/signup).

For step-by-step instructions on creating a model using the Clarifai Portal, 
visit the Clarifai [docs](https://docs.clarifai.com/portal-guide/portal_overview).

## Setup

To authenticate with the Clarifai API, users must first create a [Personal Access Token](https://docs.clarifai.com/getting-started/authentication/personal-access-tokens)
using the Clarifai Portal, then save that token to their Home Assistant configuration file.

## Configuration

To enable the Clarifai integration,
add the following snippet to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
clarifai:
  access_token: YOUR_ACCESS_TOKEN
```

{% configuration %}
access_token:
  description: The Personal Access Token used to access your Clarifai applications.
  required: true
  type: string
{% endconfiguration %}

## Predict Service

Once the integration has been configured, the `clarifai.predict` service can be used
to manually send images to Clarifai to be processed. The service can be used in automations,
either as a trigger or triggered by other automations. It can also be called by a script
or via the Amazon Echo. See the Home Assistant [docs](/docs/scripts/service-calls/)
for instructions on using service calls.

There are three parameters needed to make a `clarifai.predict` service call.

```yaml
app_id: YOUR_APP_ID
workflow_id: WORKFLOW_ID
entity_id: camera.<NAME>
```

{% configuration %}
app_id:
  description: The ID of the Clarifai Application to be used.
  example: 5fd9c2eca5564d1da2474be3709bfcf9
  type: string
workflow_id:
  description: The ID of the Workflow to be used.
  example: Face
  type: string
entity_id:
  description: The camera entity to scan for images.
  example: camera.front_door
  type: string
{% endconfiguration %}

## Image Processing Platform

In addition to the `clarifai.predict` service call, predictions can be made using the
`clarifai` platform for image processing. 

To setup the image processing platform, add the following to your `configuration.yaml` file.
The same parameters are required as when making a service call.

```yaml
# Example configuration.yaml entry
image_processing:
  - platform: clarifai
    app_id: 5fd9c2eca5564d1da2474be3709bfcf9
    workflow_id: Face
    scan_interval: 10000
    source:
      - entity_id: camera.door
```

The free version of the Clarifai API limits the number of requests possible 
per month. Therefore, it is strongly recommended that you limit the `scan_interval` when 
setting up an instance of this entity as detailed on the main [Image Processing component](/integrations/image_processing/) page.

## Prediction Event

When a response from a prediction is successfully returned from Clarifai, whether using
a service call or the image processing platform, the results are
saved as a `clarifai.prediction` event on the Home Assistant event bus. 

For using an event as a part of an automation, see the Home Assistant event [docs](/docs/configuration/events/).

To use the `clarifai.prediction` event as a trigger for automations, add the following to
your `configuration.yaml` file. 

```yaml
automation:
  trigger:
    platform: event
    event_type: clarifai.prediction
```