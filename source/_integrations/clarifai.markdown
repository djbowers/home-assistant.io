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

The `clarifai` integration for Home Assistant allows Clarifai's 
[world-leading computer vision AI platform](https://www.clarifai.com/) to be used within your home automation environment. 
To get started, create a free account on the [Clarifai portal](https://portal.clarifai.com/signup).

## Setup

A Clarifai [Personal Access Token (PAT)](https://docs.clarifai.com/getting-started/authentication/personal-access-tokens)
is required to setup the integration. Once the access token has been created with sufficient 
[scopes](https://docs.clarifai.com/getting-started/authentication/scopes), it can be used to setup the integration.

## Configuration

To enable the Clarifai integration,
add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
clarifai:
  access_token: YOUR_ACCESS_TOKEN
```

{% configuration %}
access_token:
  description: The Personal Access Token (PAT) used to access your Clarifai applications.
  required: true
  type: string
{% endconfiguration %}
