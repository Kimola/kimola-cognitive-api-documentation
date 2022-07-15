# Kimola Cognitive’s API documentation

  This repository contains the documentation for [Cognitive](https://cognitive.kimola.com)’s API.

  #### Contents

  - [Overview](#1-overview)
  - [Authentication](#2-authentication)
  - [Endpoints](#3-endpoints)
    - [Get Models By Secret with Tag](#31-get-models-by-secret-with-tag)
    - [Get Models By Secret](#32-get-models-by-secret)
    - [Get All Models](#33-get-all-models)
    
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

### 3.1. Get Models By Secret with Tag: 
  

 
  This endpoint provides the analysis results of a text block as a list of matching tags with their probabilities.
  
  
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
  
 ### 3.2. Get Models By Secret: 
 
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

 ### 3.3. Get All Models: 
 
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
    --header 'Authorization: Bearer 8onLyA9zvJqo4d4A8NCaAA=='
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

