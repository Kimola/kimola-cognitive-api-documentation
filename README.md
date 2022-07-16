# Kimola Cognitive’s API documentation

  This repository contains the documentation for [Kimola Cognitive](https://cognitive.kimola.com)’s API.

  #### Contents

  - [Overview](#1-overview)
  - [Authentication](#2-authentication)
  - [Endpoints](#3-endpoints)
    - [Getting Custom Model Predictions](#31-getting-custom-model-predictions)
    - [Getting Custom Model Predictions(Batch)](#32-getting-custom-model-predictionsbatch)
    - [Getting Pre-built Model Predictions](#33-getting-pre-built-model-predictions)
    - [Getting Pre-built Model Predictions (Batch)](#34-getting-pre-built-model-predictionsbatch)
    - [Getting Model Details](#35-getting-model-details)
    - [Listing All Models](#36-listing-all-models)
    
## 1. Overview

  Cognitive’s API is a JSON-based OAuth2 API. All requests are made to endpoints beginning:
  `https://api.kimola.com/v1`

  All requests must be secure, i.e. `https`, not `http`.

  #### Developer agreement

  By using Cognitive’s API, you agree to our [terms of service](https://kimola.com/terms-of-service).


## 2. Authentication

  Cognitive requires that you authenticate by sending an API Key with each request to grant you access to the API.
  Every Cognitive account has its own API Key. If you don’t have an account yet, you can register at [our website](https://kimola.com/cognitive).

  Once you have an account, you will find your API Key in Models page.

  - Sending API requests using cURL

  The key should be provided in the Authorization header:

  ```
  curl --location 
  --request GET 'https://api.kimola.com/v1/cognitive/Models' \
  --header 'Authorization: Bearer {key}'
  ```
  What is curl?\
    curl (short for "Client URL") is a command line tool that enables data transfer over various network protocols. It communicates with a web or       application server by specifying a relevant URL and the data that need to be sent or received. 

  Cognitive uses conventional HTTP response codes to indicate the success or failure of an API request.

  #####  Responses and Descriptions:

 |   CODE           |  DESCRIPTION                                                    |
 | -----------------|-----------------------------------------------------------------|
 |   200            |	The request is successfull.                                     |
 |   400            |	There is no API key on the request header.                      |
 |   401            |	There is an API key on the request header, but invalid.         |
 |   403            |	There is an API key on the request header, but expired.         |

## 3. Endpoints 

  The Cognitive Web API is based on REST principles.
  Data resources are accessed via standard HTTPS requests in UTF-8 format to an API endpoint. Where possible, Web API uses appropriate HTTP verbs for each action:

  |   METHOD         |  ACTION                                                      |
  |------------------|--------------------------------------------------------------|
  |   GET            |	Retrieves resources                                         |
  |   POST           |	Creates resources                                           |
  |   PUT            |	Changes and/or replaces resources or collections            |
  |   DELETE         |	Deletes resources                                           |
  
  Custom models are the name given to the user-created models that you access in the desktop models menu.
  Pre-built models are the predefined models listed on the gallery page.

### 3.1. Getting Custom Model Predictions: 
 
This endpoint returns all matching results when the request is sent and it provides the analysis results of a text block as a list of matching tags with their probabilities. 

  
| Parameter       | Type     | Required?            | Place                        |       Definition                                       |
| -------------   |----------|----------------------|------------------------------|--------------------------------------------------------|
| `secret`        | String   | required             | Path                         |       The Secret value of the data model.              |
| `text`          | String   | not required         | Query                        |       Text block to analyze by using the data model.   |
| `strict`        | Boolean  | not required         | Query                        |       Search in a strict mode.                         |

  ##### Example Request URL:  
   `https://api.kimola.com/v1/cognitive/Models/{secret}/tags`

  ##### Example Request URL with Parameters:  
   `https://api.kimola.com/v1/cognitive/Models/{secret}/tags?text=hello&strict=false`
  
   ##### Example Request:
  
    ```
    curl --location 
    --request GET 'https://api.kimola.com/v1/cognitive/Models/{secret key}/tags' \
    --header 'Authorization: Bearer {key}'
    ```
  
   ##### Example Response:

    ```
    [
      {
          "name": "...",
          "probability": 0.8
      },
      {
          "name": "...",
          "probability": 0.11
      },
      {
          "name": "...",
          "probability": 0.04
      }
    ]
    ```
    
   <details><summary>Request Examples in C#</summary>

    ```
      var client = new RestClient("https://api.kimola.com/v1/cognitive/Models/{secret key}/tags");
      client.Timeout = -1;
      var request = new RestRequest(Method.GET);
      request.AddHeader("Authorization", "Bearer {key}");
      IRestResponse response = client.Execute(request);
      Console.WriteLine(response.Content);
    ```
  
   </details>
  
  <details><summary>Request Examples in Python</summary>
  
  ```
    import requests

    url = "https://api.kimola.com/v1/cognitive/Models/kkGN6AfxjuR0xSfyMTPSlg%3D%3D/tags"

    payload={}
    headers = {
      'Authorization': 'Bearer 8onLyA9zvJqo4d4A8NCaAA=='
    }

    response = requests.request("GET", url, headers=headers, data=payload)

    print(response.text)

  ```
  
  </details>
  
 ### 3.2. Getting Custom Model Predictions(Batch): 
 
 This endpoint creates anew record that belongs to a data model. Each time you create a record, you are actually training the parent data model.
 Even if the id you send to this method is not null or unique, you will not get an error. Kimola does not guarantee a sequential response.
 
 
| Parameter       | Type     | Required?            | Place                  |       Definition                    |
| -------------   |----------|----------------------|------------------------|-------------------------------------|
| `secret`        | string   |  required            | Path                   | The Secret value of the data model. |

  ##### Example Request URL:  
   `https://api.kimola.com/v1/cognitive/Models/{secret}/tags`
  
  ##### Example Request:
  
  ```
  curl -X 'POST' \
    'https://api.kimola.com/v1/cognitive/Models/{secret}/tags' \
    -H 'accept: */*' \
    -H 'Authorization: Bearer {key}' \
    -H 'Content-Type: application/json-patch+json' \
    -d '[
    {
      "id": "...",
      "text": "..."
    }
  ]'
  ```
  
   ##### Example Response:
   
  ```
  [
    {
      "id": "...",
      "label": "...",
      "probability": 0.96
    }
  ]
  ```
  <details><summary>Request Examples in C#</summary>
  
  ```
    var client = new RestClient("https://api.kimola.com/v1/cognitive/Models/{secret key}/tags");
    client.Timeout = -1;
    var request = new RestRequest(Method.POST);
    request.AddHeader("Authorization", "Bearer {key}");
    request.AddHeader("Content-Type", "application/json");
    var body = @"[" + "\n" +
    @"  {" + "\n" +
    @"    ""id"": ""0""," + "\n" +
    @"    ""text"": ""I love this game""" + "\n" +
    @"  }" + "\n" +
    @"]'";
    request.AddParameter("application/json", body,  ParameterType.RequestBody);
    IRestResponse response = client.Execute(request);
    Console.WriteLine(response.Content);
  ```
  
  </details>
  
  <details><summary>Request Examples in Python</summary>
  
  ```
    import requests
    import json

    url = "https://api.kimola.com/v1/cognitive/Models/{secret key}/tags"

    payload = "[\n  {\n    \"id\": \"0\",\n    \"text\": \"I love this game\"\n  }\n]'"
    headers = {
      'Authorization': 'Bearer {key}',
      'Content-Type': 'application/json'
    }

    response = requests.request("POST", url, headers=headers, data=payload)

    print(response.text)

  ```
  
  </details>
  
 
 ### 3.3. Getting Pre-Built Model Predictions: 
 
 This method provides the analysis result of a text block top most matching tag with its probability.
 
| Parameter       | Type     | Required?            | Place                  |       Definition                    |
| -------------   |----------|----------------------|------------------------|-------------------------------------|
| `secret`        | string   |  required            | Path                   | The Secret value of the data model. |

  ##### Example Request URL:  
   `https://api.kimola.com/v1/cognitive/Models/{secret}/tags`
  
  ##### Example Request:
  
  ```
  curl -X 'POST' \
    'https://api.kimola.com/v1/cognitive/Models/{secret}/tags' \
    -H 'accept: */*' \
    -H 'Authorization: Bearer {key}' \
    -H 'Content-Type: application/json-patch+json' \
    -d '[
    {
      "id": "...",
      "text": "..."
    }
  ]'
  ```
  
   ##### Example Response:
   
  ```
  [
    {
      "id": "...",
      "label": "...",
      "probability": 0.96
    }
  ]
  ```
  <details><summary>Request Examples in C#</summary>
  
  ```
    var client = new RestClient("https://api.kimola.com/v1/cognitive/Models/{secret key}/tags");
    client.Timeout = -1;
    var request = new RestRequest(Method.POST);
    request.AddHeader("Authorization", "Bearer {key}");
    request.AddHeader("Content-Type", "application/json");
    var body = @"[" + "\n" +
    @"  {" + "\n" +
    @"    ""id"": ""0""," + "\n" +
    @"    ""text"": ""I love this game""" + "\n" +
    @"  }" + "\n" +
    @"]'";
    request.AddParameter("application/json", body,  ParameterType.RequestBody);
    IRestResponse response = client.Execute(request);
    Console.WriteLine(response.Content);
  ```
  
  </details>
  
  <details><summary>Request Examples in Python</summary>
  
  ```
    import requests
    import json

    url = "https://api.kimola.com/v1/cognitive/Models/{secret key}/tags"

    payload = "[\n  {\n    \"id\": \"0\",\n    \"text\": \"I love this game\"\n  }\n]'"
    headers = {
      'Authorization': 'Bearer {key}',
      'Content-Type': 'application/json'
    }

    response = requests.request("POST", url, headers=headers, data=payload)

    print(response.text)

  ```
  
  </details>
  
 
 
 ### 3.4. Getting Pre-Built Model Predictions(Batch): 
 
 This method provides the analysis results of a text block as a list of matching tags with their probabilities. 
 
| Parameter       | Type            | Required?            | Place                  |       Definition                               |
| -------------   |-----------------|----------------------|------------------------|------------------------------------------------|
| `code`          | string($uuid)   |  required            | Path                   | The Code value of the pre-built model.         |
| `language`      | string          |  required            | Path                   | the language you want to see the result in     |
| `text`          | string          |  not required        | Query                  | Text block to analyze by using the data model. |
| `strict`        | boolean         |  not required        | Query                  |                                                |



  ##### Example Request URL:  
   `https://api.kimola.com/v1/cognitive/Models/{secret}/tags`
  
  ##### Example Request:
  
  ```
  curl -X 'GET' \
  'https://api.kimola.com/v1/cognitive/Models/{code}/en/tags?text=hello \
  -H 'accept: */*' \
  -H 'Authorization: Bearer {key}'
  ```
  
   ##### Example Response:
   
  ```
[
  {
    "name": "...",
    "probability": 0.92
  },
  {
    "name": "...",
    "probability": 0.07
  },
  {
    "name": "...",
    "probability": 0.01
  }
]
  ```
  <details><summary>Request Examples in C#</summary>
  
  ```
  var client = new RestClient("https://api.kimola.com/v1/cognitive/Models/{code}/en/tags?text=hello");
  client.Timeout = -1;
  var request = new RestRequest(Method.POST);
  request.AddHeader("Authorization", "Bearer {key}");
  IRestResponse response = client.Execute(request);
  Console.WriteLine(response.Content);
  ```
  
  </details>
  
  <details><summary>Request Examples in Python</summary>
  
  ```
  import requests

  url = "https://api.kimola.com/v1/cognitive/Models/{code}/en/tags?text=hello"

  payload={}
  headers = {
    'Authorization': 'Bearer {key}'
  }

  response = requests.request("POST", url, headers=headers, data=payload)

  print(response.text)


  ```
  
  </details>
  
 
 
 ### 3.5. Getting Model Details: 
 
  This endpoint provides details of a data model.
  
  
| Parameter       | Type     | Required?            | Place                  |       Definition                    |
| -------------   |----------|----------------------|------------------------|-------------------------------------|
| `secret`        | string   |  required            | Path                   | The Secret value of the data model. |
  
  ##### Example Request URL:  
   `https://api.kimola.com/v1/cognitive/Models/{secret} `
  
  ##### Example Request:
  ```
  curl --location 
  --request GET 'https://api.kimola.com/v1/cognitive/Models/{secret key}' \
  --header 'Authorization: Bearer {key}'
  ```
  
   ##### Example Response:
  ```
  {
  "isReady": true,
  "id": ...,
  "language": "en",
  "languageName": "English",
  "name": "...",
  "category": "...",
  "algorithm": "...",
  "accuracy": 0.87,
  "recordCount": 5049,
  "trainedDate": "...",
  "modifiedDate": "...",
  "createdDate": "..."
  }
  ```

  <details><summary>Request Examples in C#</summary>
  
  ```
  var client = new RestClient("https://api.kimola.com/v1/cognitive/Models/{secret key}");
  client.Timeout = -1;
  var request = new RestRequest(Method.GET);
  request.AddHeader("Authorization", "Bearer {key}");
  IRestResponse response = client.Execute(request);
  Console.WriteLine(response.Content);
  ```
  
  </details>
  
  <details><summary>Request Examples in Python</summary>
  
  ```
    import requests

    url = "https://api.kimola.com/v1/cognitive/Models/{secret key}"

    payload={}
    headers = {
      'Authorization': 'Bearer {key}'
    }

    response = requests.request("GET", url, headers=headers, data=payload)

    print(response.text)
  
  ```
  
  </details>

 ### 3.6. Listing All Models: 
 
   This endpoints provides the list of data models.
  - The base URI for all Web API requests is: `https://api.kimola.com/v1/cognitive/Models`
  
 #### Pagination:
   Endpoint support a way of paging the models, taking an pageSize and pageIndex as query parameters. 
   Any API response which contains multiple resources supports several common query parameters to handle paging through the response data:\
   The application can use to indicate the page size (the number of items to return in the response) and the page index(index of the first item you want    results for). 
  
   | Parameter       | Type     | Required?            |              Place                              |
   | -------------   |----------|----------------------|-------------------------------------------------|
   | `pageSize`      | Int32    | not required         |               Query                             |
   | `pageIndex`     | Int32    | not required         |               Query                             |
   
  ##### Example Request URL:  
   `https://api.kimola.com/v1/cognitive/Models`
   
  ##### Example Request URL with Parameters:  
   `https://api.kimola.com/v1/cognitive/Models?pageIndex=0&pageSize=10`
  
  ##### Example of Request with Pagination:
   
   ```
   curl --location 
   --request GET 'https://api.kimola.com/v1/cognitive/Models/?pageSize=10&pageIndex=1' \
    --header 'Authorization: Bearer {key}'
   ```
   
  ##### Example Request:
  ```
  curl --location 
  --request GET 'https://api.kimola.com/v1/cognitive/Models' \
  --header 'Authorization: Bearer {key}'
  ```
  
   ##### Example Response:
  ```
  
  [
  {
    "isReady": true,
    "id": ...,
    "language": "en",
    "languageName": "English",
    "name": "...",
    "category": "...",
    "algorithm": "...",
    "accuracy": 0.87,
    "isOwner": true,
    "recordCount": ...,
    "trainedDate": "...",
    "modifiedDate": "...",
    "createdDate": "..."
  },
  ...
  ]
  
  ```

  <details><summary>Request Examples in C#</summary>
  
  ```
  var client = new RestClient("https://api.kimola.com/v1/cognitive/Models");
  client.Timeout = -1;
  var request = new RestRequest(Method.GET);
  request.AddHeader("Authorization", "Bearer {key}");
  IRestResponse response = client.Execute(request);
  Console.WriteLine(response.Content);
  ```
  
  </details>
  
  <details><summary>Request Examples in Python</summary>
  
  ```
    import requests

    url = "https://api.kimola.com/v1/cognitive/Models"

    payload={}
    headers = {
      'Authorization': 'Bearer {key}'
    }

    response = requests.request("GET", url, headers=headers, data=payload)

    print(response.text)
  
  ```
  
  </details>
  
  When the result comes, top match result comes.
  It lists all the categories that may contain the word and the probability of being in those categories.
  
  Even if the id you send to this method is not null or unique, you will not get an error. Kimola does not guarantee a sequential response. Please make sure that the id you enter is unique.
  
  Kimola uses the languages it supports, the notation of ISO [639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) codes with 2-letter notation. ISO 639 is a standardized nomenclature used to classify languages. Each language is assigned a two-letter (639-1).
  For example, when you want to choose Spanish, es, which corresponds to 2 letters, should be selected as the language code.
  

  Batch input size must be lower than 100.
