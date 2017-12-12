---
title:Python Quickstart for Azure Cognitive Services, Bing News Search API | Microsoft Docs
description:Get information and code samples to help you quickly get started using the Bing News Search API in Microsoft Cognitive Services on Azure.
services:cognitive-services
author:jerrykindall
ms.service:cognitive-services
ms.technology:bing-search
ms.topic:article
ms.date:9/21/2017
ms.author:v-jerkin
---

# Quickstart for Bing News Search API with Python

This article shows you how use the Bing News Search API, part of Microsoft Cognitive Services on Azure. While this article employs Python, the API is a RESTful Web service compatible with any programming language that can make HTTP requests and parse JSON. 

The example code was written to run under Python 3.

Refer to the [API reference](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) for technical details about the APIs.

## Prerequisites

You must have a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with **Bing Search APIs**. The [free trial](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is sufficient for this quickstart. You will need the access key provided when you activate your free trial, or you may use a paid subscription key from your Azure dashboard.

## Bing News search

The [Bing News Search API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-news-api-v7-reference) returns news results from the Bing search engine.

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
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

We will now search Bing for news about Microsoft!


```python
search_term = "Microsoft"
```

The following block uses the `requests` library in Python to call out to the Bing seach APIs and return the results as a JSON object. Observe that we pass in the API key via the `headers` dictionary and the search term via the `params` dictionary. To see the full list of options that can be used to filter search results, please see the [REST API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-news-api-v7-reference) documentation.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

The `search_results` object contains the relevant new articles along with rich metadata. For example, we can extract the descriptions of the articles using the following line of code.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

We can then render the results in a table with the search keyword highlighted in **bold**.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```




<table><tr><td>In late October, <b>Microsoft</b> reported first quarter revenue that rose 11.9% to $24.54 billion – beating consensus estimates by $980 million – and net income of 84 cents per share beat consensus estimates by 12 cents per share. Shares responded by jumping ...</td></tr>
<tr><td>In-N-Out is one of the best companies to work for in the US, according to employees. The burger chain earned the No. 4 spot on Glassdoor&#39;s list of the best places to work in 2018. In-N-Out beat out tech giants like Google and <b>Microsoft</b> in the ranking.</td></tr>
<tr><td>Today, at the Qualcomm Snapdragon Tech Summit, <b>Microsoft</b> and Qualcomm officially unveiled the first ARM-powered Windows 10 laptops. These devices, referred to as Always Connected PCs, are always on, always connected (via LTE), and promise &quot;incredible ...</td></tr>
<tr><td>“<b>Microsoft</b> Monday” is a weekly column that focuses on all things <b>Microsoft</b>. This week, <b>Microsoft</b> Monday includes details about the major Redmond campus expansion, Edge for Android and iOS, a job listing hinting at a Surface with a Snapdragon 845 ...</td></tr>
<tr><td>Could Windows Phone return? Maybe. At a Q&amp;A with <b>Microsoft</b> and Qualcomm execs at Qualcomm&#39;s Snapdragon Summit, Qualcomm&#39;s Miguel Nunes said &quot;different form factors&quot; are coming for Windows 10 on Snapdragon, and <b>Microsoft</b>&#39;s Erin Chapple didn&#39;t count out the ...</td></tr>
<tr><td>A big part of <b>Microsoft</b>&#39;s project is to introduce a massive green roof and put its acres of surface parking under a soccer field. <b>Microsoft</b>&#39;s soon-to-be revamped Silicon Valley office will be designed to entirely remove its reliance on water from municipal ...</td></tr>
<tr><td>It&#39;s a new era for <b>Microsoft</b> (NASDAQ:MSFT). Under prior CEO Steve Ballmer, the company experienced a lost decade for failing to adjust to the shift to mobile and falling behind various competitors -- notably Apple in smartphones and tablets, Alphabet in ...</td></tr>
<tr><td><b>Microsoft</b> continued its support for the Kubernetes open source project today, with the unveiling of several enhancements to systems that are designed to make it easier for developers to connect that system with the tech titan’s Azure cloud platform.</td></tr>
<tr><td>Being transported to a virtual world can be fantastic -- until you realize what you&#39;ve left behind. No email, no instant messages from friends or family, no TV in the background, no streaming tunes, no Tweets or Snaps or Likes. The Fear of Missing Out ...</td></tr>
<tr><td>After a long wait time, <b>Microsoft</b>’s Office apps are now finally available on all Chromebooks — though it’ll be useless if you have a larger Chromebook. Office apps have long been corralled to only a particular set of Chromebooks, with no apparent ...</td></tr></table>



## JSON response

For completeness, a sample response is shown below. To limit the length of the JSON, only a single result is shown, and other parts of the response have been truncated. 
```json
{
   "_type": "News",
   "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft",
   "totalEstimatedMatches": 36,
   "sort": [
      {
         "name": "Best match",
         "id": "relevance",
         "isSelected": true,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft"
      },
      {
         "name": "Most recent",
         "id": "date",
         "isSelected": false,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft&sortby=date"
      }
   ],
   "value": [
      {
         "name": "Microsoft to open flagship London brick-and-mortar retail store",
         "url": "http:\/\/www.zdnet.com\/article\/microsoft-to-open-flagship-london-brick-and-mortar-retail-store\/",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.F9E4A49EC010417D2022A5BEB47CD95A&pid=News",
               "width": 220,
               "height": 146
            }
         },
         "description": "After years of rumors about Microsoft opening a brick-and-mortar retail store in London, it's actually happening. On September 21, Microsoft officials announced the company will be opening its first European Microsoft Store. The store will be on Regent ...", "about": [{"readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85", "name": "Microsoft"}, {"readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/8e0ba7b6-4225-fa8a-6369-1b5294e602a5", "name": "London"}], "provider": [{"_type": "Organization", "name": "ZDNet"}], "datePublished": "2017-09-21T21:16:00.0000000Z", "category": "ScienceAndTechnology"}, {"name": "Ford Brings Mixed Reality Into Car Design With Microsoft HoloLens", "url": "http:\/\/www.eweek.com\/enterprise-apps\/ford-brings-mixed-reality-into-car-design-with-microsoft-hololens", "image": {"thumbnail": {"contentUrl": "https:\/\/www.bing.com\/th?id=ON.8D2B4F03E769BBD0C6C969BEBFF865BC&pid=News", "width": 700, "height": 466}}, "description": "Coming off a year-long pilot program, Microsoft and Ford have taken the wraps off a collaboration between the two companies that will help the automaker's designers get their creations on the road faster by using HoloLens. Microsoft's HoloLens is a ...", "about": [{"readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/35afa8a9-05af-bda7-8819-13e11ca5d726", "name": "Ford Motor Company"}, {"readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/eea5a113-6354-c784-d386-e6245314128d", "name": "Faster"}, {"readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85", "name": "Microsoft"}], "provider": [{"_type": "Organization", "name": "eWeek"}], "datePublished": "2017-09-21T23:52:00.0000000Z", "category": "ScienceAndTechnology"}, {"name": "Has the pace of Windows 10 upgrades stalled completely? Clues from Microsoft suggest it has", "url": "https:\/\/betanews.com\/2017\/09\/20\/stalled-windows-10-upgrades\/", "image": {"thumbnail": {"contentUrl": "https:\/\/www.bing.com\/th?id=ON.6546221F720801AC93BD6A968F6C84B8&pid=News", "width": 640, "height": 549}}, "description": "When Windows 10 was still (officially) free, and Microsoft was forcing it onto systems against user wishes, the operating system’s market share growth was impressive. In no time at all it shot past Windows XP and Windows 8.x. SEE ALSO: Microsoft reduces ...", "about": [{"readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/16aeb6d9-9098-0a40-4970-8e46a4fcee12", "name": "Microsoft Windows"}, {"readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85", "name": "Microsoft"}], "provider": [{"_type": "Organization", "name": "Beta News"}], "datePublished": "2017-09-21T23:27:00.0000000Z", "category": "ScienceAndTechnology"}, {"name": "Microsoft will reportedly open a new flagship retail store in the centre of London", "url": "http:\/\/www.businessinsider.com\/microsoft-will-reportedly-open-a-new-flagship-retail-store-in-the-centre-of-london-2017-9", "image": {"thumbnail": {"contentUrl": "https:\/\/www.bing.com\/th?id=ON.837248994470A7BBA13392CA9065D919&pid=News", "width": 700, "height": 525}}, "description": "Microsoft is reportedly coming closer to a UK retail launch, according to RetailWeek (we first saw the report via The Verge), with a new, flagship store set to open in one of London's most central locations, Oxford Circus. The building Microsoft is ...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/8e0ba7b6-4225-fa8a-6369-1b5294e602a5",
               "name": "London"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "name": "Microsoft"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Business Insider"
            }
         ],
         "datePublished": "2017-09-21T17:53:00.0000000Z",
         "category": "ScienceAndTechnology"
      },
      {
         "name": "Microsoft adds Availability Zones to its Azure cloud platform",
         "url": "https:\/\/venturebeat.com\/2017\/09\/21\/microsoft-adds-availability-zones-to-its-azure-cloud-platform\/",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0AE7595B9720E7F9A5859834715D479C&pid=News",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft has begun adding Availability Zones to its Azure cloud platform, providing customers with an additional tool to make sure their applications stay operational. The zones are sets of data centers within a single cloud region that are geographically ...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/cf3abf7d-e379-2693-f765-6da6b9fa9149",
               "name": "Windows Azure"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/9cdd061c-1fae-d046-08ed-9ae1879b7aae",
               "name": "Cloud"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "VentureBeat"
            }
         ],
         "datePublished": "2017-09-21T09:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      },
      {
         "name": "Microsoft confirms plans for a new flagship store in Regent Street opposite Apple",
         "url": "https:\/\/techcrunch.com\/2017\/09\/21\/microsoft-confirms-plans-for-a-new-flagship-store-in-regent-street-opposite-apple\/",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.08997A8F350BD805254EA0C77508E150&pid=News",
               "width": 700,
               "height": 466
            }
         },
         "description": "Shopping may be turning into an increasingly virtual experience, with people buying goods online and through apps, but there is no denying the power of a physical in-store experience — a lesson that Microsoft is taking to heart. Today the company ...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/6fa57c29-e813-4a1d-7fba-3541ca4c1dc3",
               "name": "Apple Inc."
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/e0790b73-6baf-f702-218a-1191248f8753",
               "name": "Flagship"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "TechCrunch"
            }
         ],
         "datePublished": "2017-09-21T23:53:00.0000000Z",
         "category": "ScienceAndTechnology"
      },
      {
         "name": "Microsoft announces Ford AR expansion; new retail store coming to London?",
         "url": "http:\/\/www.msn.com\/en-us\/money\/other\/microsoft-announces-ford-ar-expansion-new-retail-store-coming-to-london\/ar-AAsjc8T",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.7412FC3D76EA7F8C08F5CC2B78122712&pid=News",
               "width": 700,
               "height": 525
            }
         },
         "description": "Microsoft (NASDAQ:MSFT) announces Ford has expanded testing of Microsoft’s HoloLens headsets to help designers create model vehicles on top of an existing vehicle using augmented reality. AR headsets sell far fewer units than the more consumer-friendly ...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/8e0ba7b6-4225-fa8a-6369-1b5294e602a5",
               "name": "London"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/87153d87-9bb0-166a-3d56-613bdc274e1b",
               "name": "Argentina"
            }
         ],
         "mentions": [
            {
               "name": "Microsoft"
            },
            {
               "name": "London"
            },
            {
               "name": "Argentina"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Seeking Alpha on MSN.com"
            }
         ],
         "datePublished": "2017-09-21T14:02:23.0000000Z",
         "category": "ScienceAndTechnology"
      },
      {
         "name": "Will Microsoft’s Xbox One X Enhanced Program Actually Make Games Look And Play Better?",
         "url": "http:\/\/www.techtimes.com\/articles\/213673\/20170921\/will-microsoft-s-xbox-one-x-enhanced-program-actually-make-games-look-and-play-better.htm",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.FEAEB6F53A19B8A1887C5206A3B67ADC&pid=News",
               "width": 650,
               "height": 478
            }
         },
         "description": "The major appeal of Microsoft's soon-to-be-released flagship gaming machine comes from its ability to play 4K games natively. That's the promise gamers hold onto when shelling out $500. But despite its rapidly increasing adoption, 4K TVs remain a luxury ...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/446bb4df-4999-4243-84c0-74e0f6c60e75",
               "name": "Xbox One"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/18ddabcb-d74a-d8ca-08fe-631a48642ea5",
               "name": "Michigan Technological University"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Tech Times"
            }
         ],
         "datePublished": "2017-09-21T15:29:00.0000000Z",
         "category": "ScienceAndTechnology"
      },
      {
         "name": "Microsoft: Windows getting more stable, faster, and lasting longer on battery",
         "url": "https:\/\/arstechnica.com\/gadgets\/2017\/09\/microsoft-windows-getting-more-stable-faster-and-lasting-longer-on-battery\/",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0B4338C83DD7C85A385EF0F66DFB5984&pid=News",
               "width": 640,
               "height": 400
            }
         },
         "description": "Windows 10 is getting better and better, Microsoft insists, as it works to build confidence in the operating system in the run up to the next major update. The company says that the Creators Update (version 1703) has seen a 39 percent drop in driver and ...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/16aeb6d9-9098-0a40-4970-8e46a4fcee12",
               "name": "Microsoft Windows"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/57039f6d-eab0-94cd-33cf-a20bf6fdf8a9",
               "name": "Battery"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Ars Technica"
            }
         ],
         "datePublished": "2017-09-21T00:26:00.0000000Z"
      },
      {
         "name": "Microsoft opens standard Xbox One X preorders worldwide",
         "url": "https:\/\/www.theverge.com\/2017\/9\/20\/16336406\/microsoft-xbox-one-x-standard-worldwide-preorder-now",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.CB8912F5A1D6EFFAFE4C50A3FF86AA66&pid=News",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft today announced worldwide preorder availability for the standard version of its new Xbox One X console. The device, first unveiled back at E3 in June, has only been available up until now as the Project Scorpio special edition, preorders for ...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/446bb4df-4999-4243-84c0-74e0f6c60e75",
               "name": "Xbox One"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "name": "Microsoft"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "The Verge"
            }
         ],
         "datePublished": "2017-09-20T07:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      }
   ]
}
```

## Next steps

> [!div class="nextstepaction"]
> [Paging news](paging-news.md)
> [Using decoration markers to highlight text](hit-highlighting.md)

## See also 

 [Searching the web for news](search-the-web.md)  
 [Try it](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)