---
title:Python Quickstart for Azure Cognitive Services, Bing Video Search API | Microsoft Docs
description:Get information and code samples to help you quickly get started using the Bing Video Search API in Microsoft Cognitive Services on Azure.
services:cognitive-services
author:jerrykindall
ms.service:cognitive-services
ms.technology:bing-search
ms.topic:article
ms.date:9/21/2017
ms.author:v-jerkin
---

# Quickstart for Bing Video Search API with Python

This article shows you how use the Bing Video Search API, part of Microsoft Cognitive Services on Azure. While this article employs Python, the API is a RESTful Web service compatible with any programming language that can make HTTP requests and parse JSON. 

Refer to the [API reference](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-video-api-v7-reference) for technical details about the APIs.

## Prerequisites

You must have a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with **Bing Search APIs**. The [free trial](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is sufficient for this quickstart. You will need the access key provided when you activate your free trial, or you may use a paid subscription key from your Azure dashboard.

## Bing Video search

The [Bing Video Search API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-video-api-v7-reference) returns video results from the Bing search engine.

1. Create a new Python project in your favorite IDE or editor.
2. Add the code provided below.
3. Replace the `subscriptionKey` value with an access key valid for your subscription.
4. Run the program.

Please set `subscriptionKey` below to your API key for the Bing API service.


```python
subscription_key = "9f6105c9e84b4e65b6119769ee55c545"
assert subscription_key
```

Next, please verify that the `search_url` endpoint is correct. At this writing, only one endpoint is used for Bing search APIs.  In the future, regional endpoints may be available.  If you encounter unexpected authorization errors, double-check this value against the endpoint for your Bing search instance in your Azure dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

We will now search Bing for videos of kittens!


```python
search_term = "kittens"
```

The following block uses the `requests` library in Python to call out to the Bing seach APIs and return the results as a JSON object. Observe that we pass in the API key via the `headers` dictionary and the search term via the `params` dictionary. To see the full list of options that can be used to filter search results, please see the [REST API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-video-api-v7-reference) documentation.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

The `search_results` object contains the relevant videos along with rich metadata. To view one of the videos, we can use its `embedHtml` property and insert it into an `IFrame`.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"])
```




<iframe width="1280" height="720" src="https://www.youtube.com/embed/8HVWitAW-Qg?autoplay=1" frameborder="0" allowfullscreen></iframe>



## Next steps

> [!div class="nextstepaction"]
> [Paging videos](paging-videos.md)
> [Resizing and cropping thumbnail images](resize-and-crop-thumbnails.md)

## See also 

 [Searching the web for videos](search-the-web.md)
 [Try it](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
