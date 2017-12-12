---
title: Call and response - Python Quickstart for Azure Cognitive Services, Bing Web Search API | Microsoft Docs
description: Get information and code samples to help you quickly get started using the Bing Web Search API in Microsoft Cognitive Services on Azure.
services: cognitive-services
author: jerrykindall
ms.service: cognitive-services
ms.technology: bing-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
---

# Call and response: your first Bing Web Search query in Python

The Bing Web Search API provides a experience similar to Bing.com/Search by returning search results that Bing determines are relevant to the user's query. The results may include Web pages, images, videos, news, and entities, along with related search queries, spelling corrections, time zones, unit conversion, translations, and calculations. The kinds of results you get are based on their relevance and the tier of the Bing Search APIs to which you subscribe.

Refer to the [API reference](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-web-api-v7-reference) for technical details about the APIs.

## Prerequisites
You must have a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with **Bing Search APIs**. The [free trial](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is sufficient for this quickstart. You need the access key provided when you activate your free trial, or you may use a paid subscription key from your Azure dashboard.

## Running the application

Please set `subscriptionKey` below to your API key for the Bing API service.


```python
subscription_key = "9f6105c9e84b4e65b6119769ee55c545"
assert subscription_key
```

Next, please verify that the `search_url` endpoint is correct. At this writing, only one endpoint is used for Bing search APIs.  In the future, regional endpoints may be available.  If you encounter unexpected authorization errors, double-check this value against the endpoint for your Bing search instance in your Azure dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

We will now search Bing for Microsoft Cognitive Services.


```python
search_term = "Microsoft Cognitive Services"
```

The following block uses the `requests` library in Python to call out to the Bing seach APIs and return the results as a JSON object. Observe that we pass in the API key via the `headers` dictionary and the search term via the `params` dictionary. To see the full list of options that can be used to filter search results, please see the [REST API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-web-api-v7-reference) documentation.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

The `search_results` object contains the research results along with rich metadata such as related queries and pages. We can format the top pages returned by the query using the following lines of code


```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```




<table><tr>
                       <td><a href="https://www.microsoft.com/cognitive-services"><b>Microsoft</b> <b>Cognitive</b> <b>Services</b></a></td>
                       <td>Knock down barriers between you and your ideas. Enable natural and contextual interaction with tools that augment users&#39; experiences via the power of machine-based AI. Plug them in and bring your ideas to life.</td>
                     </tr>
<tr>
                       <td><a href="https://www.microsoft.com/en-us/trustcenter/cloudservices/cognitiveservices"><b>Microsoft</b> Trust Center | <b>Microsoft Cognitive Services</b></a></td>
                       <td><b>Microsoft Cognitive Services</b> is a collection of intelligent APIs that allow systems to understand and interpret people’s needs by using natural methods of ...</td>
                     </tr>
<tr>
                       <td><a href="https://azure.microsoft.com/en-us/try/cognitive-services/"><b>Cognitive</b> Service Try experience | <b>Microsoft Azure</b></a></td>
                       <td><b>Microsoft Cognitive Services</b> Try experience lets you build apps with powerful algorithms using just a few lines of code through a 30 day trial.</td>
                     </tr>
<tr>
                       <td><a href="https://docs.microsoft.com/en-us/azure/cognitive-services/Welcome">What is <b>Microsoft Cognitive Services</b>? | <b>Microsoft</b> Docs</a></td>
                       <td><b>Microsoft Cognitive Services</b> is a set of APIs, SDKs, and <b>services</b> that you can use with <b>Microsoft</b> Azure that make applications more intelligent, engaging, and ...</td>
                     </tr>
<tr>
                       <td><a href="https://customers.microsoft.com/en-us/search?sq=%22Microsoft%20Cognitive%20Services%22&ff=&p=0&so=story_publish_date%20desc"><b>Microsoft Cognitive Services</b> - customers.<b>microsoft</b>.com</a></td>
                       <td><b>Microsoft</b> customer stories. See how <b>Microsoft</b> tools help companies run their business.</td>
                     </tr>
<tr>
                       <td><a href="https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236"><b>Microsoft Cognitive Services</b></a></td>
                       <td>Face - Detect. Detect human faces in an image and returns face locations, and optionally with faceIds, landmarks, and attributes. Optional parameters for returning ...</td>
                     </tr>
<tr>
                       <td><a href="https://msdn.microsoft.com/en-us/magazine/mt742868.aspx"><b>Cognitive Services</b> - <b>msdn.microsoft.com</b></a></td>
                       <td>This article explains how you can recognize face attributes and emotions using the new Face and Emotion APIs from the <b>Microsoft Cognitive Services</b> in Xamarin.Forms by ...</td>
                     </tr>
<tr>
                       <td><a href="https://news.microsoft.com/cognitive/"><b>Microsoft Cognitive Services</b></a></td>
                       <td>The machine-learned smarts that enable <b>Microsoft</b>’s Skype Translator, Bing and Cortana to accomplish tasks such as translating conversations, compiling knowledge and ...</td>
                     </tr>
<tr>
                       <td><a href="https://docs.microsoft.com/en-us/azure/cognitive-services/Computer-vision/Home"><b>Computer Vision API</b> for <b>Microsoft Cognitive Services</b> ...</a></td>
                       <td>Use advanced algorithms in the <b>Computer Vision API</b> to help you process images and return information in <b>Microsoft Cognitive Services</b>.</td>
                     </tr></table>



## Next steps

> [!div class="nextstepaction"]
> [Bing Web search single-page app tutorial](../tutorial-bing-web-search-single-page-app.md)

## See also 

[Bing Web Search overview](../overview.md)  
[Try it](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Get a free trial access key](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Bing Web Search API reference](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
