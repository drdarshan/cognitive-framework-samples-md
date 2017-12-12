---
title:Python Quickstart for Azure Cognitive Services, Text Analytics API | Microsoft Docs
description:Get information and code samples to help you quickly get started using the Text Analytics API in Microsoft Cognitive Services on Azure.
services:cognitive-services
author:luiscabrer
ms.service:cognitive-services
ms.technology:text-analytics
ms.topic:article
ms.date:08/24/2017
ms.author:luisca
---

# Quickstart for Text Analytics API with Python 
<a name="HOLTop"></a>

This article shows you how to [detect language](#Detect), [analyze sentiment](#SentimentAnalysis), and [extract key phrases](#KeyPhraseExtraction) using the [Text Analytics APIs](//go.microsoft.com/fwlink/?LinkID=759711) with Python.

Refer to the [API definitions](//go.microsoft.com/fwlink/?LinkID=759346) for technical documentation for the APIs.

## Prerequisites

You must have a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with **Text Analytics API**. You can use the **free tier for 5,000 transactions/month** to complete this quickstart.

You must also have the [endpoint and access key](../How-tos/text-analytics-how-to-access-key.md) that was generated for you during sign up. 

To continue with this walkthrough, please replace `subscription_key` below with a valid subscription key that you obtained earlier.


```python
subscription_key="bc96c8f6806346878e0103e08d71f7f5"
```

Next, verify that the region in `text_analytics_base_url` corresponds to the one you used when setting up the service. If you are using a free trial key, you are already good to go!


```python
text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/"
```

<a name="Detect"></a>

## Detect languages

The Language Detection API detects the language of a text document, using the [Detect Language method](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7). The service endpoint of the language detection API for your region is shown below.


```python
language_api_url = text_analytics_base_url + "languages"
language_api_url
```




    'https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/languages'



The payload to the API consists of a list of `documents`, each of which in turn contains an `id` and a `text` attribute. The `text` attribute stores the text to be analyzed. 

Please replace the `documents` dictionary below with any other text for language detection. 


```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

Next, we can call out to the language detection API using the `requests` library in Python to determine the language in the documents.


```python
import requests
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
languages
```




    {'documents': [{'detectedLanguages': [{'iso6391Name': 'en',
         'name': 'English',
         'score': 1.0}],
       'id': '1'},
      {'detectedLanguages': [{'iso6391Name': 'es',
         'name': 'Spanish',
         'score': 1.0}],
       'id': '2'},
      {'detectedLanguages': [{'iso6391Name': 'zh_chs',
         'name': 'Chinese_Simplified',
         'score': 1.0}],
       'id': '3'}],
     'errors': []}



We can now post-process the JSON data to render it as a table:


```python
from IPython.display import HTML
table = []
for document in languages["documents"]:
    text = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    col_2 = []
    for lang in document["detectedLanguages"]:
        col_2.append("<tr><td>{0}</td><td>{1}</td></tr>".format(lang["name"], lang["score"]))
    col_2 = "<table><tr><th>Language</th><th>Score</th></tr>{0}</table>".format("\n".join(col_2))
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, col_2))
HTML("<table><tr><th>Text</th><th>Detected languaged</th></tr>{0}</table>".format("\n".join(table)))
```




<table><tr><th>Text</th><th>Detected languaged</th></tr><tr><td>This is a document written in English.</td><td><table><tr><th>Language</th><th>Score</th></tr><tr><td>English</td><td>1.0</td></tr></table></td>
<tr><td>Este es un document escrito en Español.</td><td><table><tr><th>Language</th><th>Score</th></tr><tr><td>Spanish</td><td>1.0</td></tr></table></td>
<tr><td>这是一个用中文写的文件</td><td><table><tr><th>Language</th><th>Score</th></tr><tr><td>Chinese_Simplified</td><td>1.0</td></tr></table></td></table>



<a name="SentimentAnalysis"></a>

## Analyze sentiment

The Sentiment Analysis API detexts the sentiment of a set of text records, using the [Sentiment method](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9). The following example scores two documents, one in English and another in Spanish.

The service endpoint for sentiment analysis is available for your region via the following URL:


```python
sentiment_api_url = text_analytics_base_url + "sentiment"
sentiment_api_url
```




    'https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment'



As with the language detection example, we provide the service with a dictionary with a `documents` key that consists of a list of documents. Each document is a tuple consisting of the `id`, the `text` to be analyzed and the `language` of the text. You can use the language detection API from the previous section to populate this field. 


```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

We can now use the sentiment API to analyze the documents for their sentiments.


```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
sentiments
```




    {'documents': [{'id': '1', 'score': 0.9997933506965637},
      {'id': '2', 'score': 9.506940841674805e-06},
      {'id': '3', 'score': 0.7456425428390503},
      {'id': '4', 'score': 0.334433376789093}],
     'errors': []}



The sentiment score for a document is between $0$ and $1$, with a higher score indicating a more positive sentiment.

<a name="KeyPhraseExtraction"></a>

## Extract key phrases

The Key Phrase Extraction API extracts key-phrases from a text document, using the [Key Phrases method](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). This section of the walkthrough extracts key phrases for both English and Spanish documents.

The service endpoint for the key-phrase extraction service is accessed via the following URL:


```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
key_phrase_api_url
```




    'https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases'



We use the same collection of documents that we used earlier for sentiment analysis.


```python
documents
```




    {'documents': [{'id': '1',
       'language': 'en',
       'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
      {'id': '2',
       'language': 'en',
       'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},
      {'id': '3',
       'language': 'es',
       'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},
      {'id': '4',
       'language': 'es',
       'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}]}




```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
key_phrases
```




    {'documents': [{'id': '1',
       'keyPhrases': ['wonderful experience', 'staff', 'rooms']},
      {'id': '2', 'keyPhrases': ['food', 'terrible time', 'hotel', 'staff']},
      {'id': '3', 'keyPhrases': ['Monte Rainier', 'caminos']},
      {'id': '4', 'keyPhrases': ['carretera', 'tráfico', 'día']}],
     'errors': []}



We can now format the JSON object into a table using the block of code below:


```python
from IPython.display import HTML
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```




<table><tr><th>Text</th><th>Key phrases</th></tr><tr><td>I had a wonderful experience! The rooms were wonderful and the staff was helpful.</td><td>wonderful experience,staff,rooms</td>
<tr><td>I had a terrible time at the hotel. The staff was rude and the food was awful.</td><td>food,terrible time,hotel,staff</td>
<tr><td>Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.</td><td>Monte Rainier,caminos</td>
<tr><td>La carretera estaba atascada. Había mucho tráfico el día de ayer.</td><td>carretera,tráfico,día</td></table>



## Next steps

> [!div class="nextstepaction"]
> [Text Analytics With Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## See also 

 [Text Analytics overview](../overview.md)  
 [Frequently asked questions (FAQ)](../text-analytics-resource-faq.md)
