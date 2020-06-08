---
title: generative.photos API Documentation
language_tabs:
  - php: Php
  - python: Python
toc_footers: []
includes: []
search: false
highlight_theme: darkula
headingLevel: 2

---

<h1 id="generative-photos-api-documentation">Introduction</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

## Overview

Base URLs:

* <a href="https://app.generative.photos/api/v0">https://app.generative.photos/api/v0</a>

Current API version is 0.1.0

Except for image upload/download, all API-endpoints expect and return JSON.

## Typical Usage

* obtain API key (email us)
* <a href="#opIdpostTeamsMeGallery">upload your image</a> and note its auto-assigned `id` from the response
* poll the <a href="#opIdgetTeamsMeGalleryDocumentid">information</a> about the uploaded image using `id` in the URL of the `GET` request
* continue polling the same endpoint until `data.faces` in the response contains information about relative face-rectangles
* <a href="#opIdputTeamsMeGalleryDocumentidFacesFaceid">create a new variant</a> using the `id` to build a URL for a `PUT` request and also note the auto-assigned `id` of the new variant from the response
* poll the <a href="#opIdgetTeamsMeGalleryDocumentid">information</a> about the newly generated variant by using variant's `id` in the URL of the `GET` request
* continue polling the same endpoint until `data.task.status` in the response is either `complete` or `failed`
* <a href="#opIdgetTeamsMeGalleryDocumentidPixelsOriginalBytes">download</a> the new image variant (as PNG) once generation is complete

Download a <a href="images/generative.photos API.postman_collection.json">Postman collection</a> and a <a href="images/generative.photos.postman_environment.json">Postman environment</a> corresponding to the sequence of steps above.

# Authentication

* API Key (jwt)
    - Parameter Name: **Authorization**, in: header. 

Please contact us to obtain or expire/refresh your API key.

Currently, the key is static and non-expiring.

<h1 id="generative-photos-api-documentation-api">Endpoints</h1>

## Get image information

<a id="opIdgetTeamsMeGalleryDocumentid"></a>

> Code samples

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => '*/*',
    'authorization' => 'string',
    
    );

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://app.generative.photos/api/v0/teams/me/gallery/{documentid}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```python
import requests
headers = {
  'Accept': '*/*',
  'authorization': 'string'
}

r = requests.get('https://app.generative.photos/api/v0/teams/me/gallery/{documentid}', params={

}, headers = headers)

print r.json()

```

`GET /teams/me/gallery/{documentid}`

This endpoint is designed to quickly return the status of the image or image-variant.

We expect your application to poll it frequently after you <a href="#opIdputTeamsMeGalleryDocumentidFacesFaceid">submit variant-generation request</a>.

For a new image-variant the typical progression of the `data.task.status` in the response is:

* `waiting`
* `inprogress`
* `complete`

In case of failure of the generative-algorithm, the progression of the `data.task.status` in the response is:

* `waiting`
* `inprogress`
* `failed` with `data.task.error.code` containing the error-code

When uploading new image, its `data.task.status` is immediately complete, but it may take a few seconds for our systems to recognize all faces on the image.

<h3 id="getteamsmegallerydocumentid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|authorization|header|string|true|none|
|documentid|path|string|true|none|

> Example responses

> 200 Response

<h3 id="getteamsmegallerydocumentid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GalleryItem](#schemagalleryitem)|

<aside class="success">
This operation requires API key
</aside>

## Download image

<a id="opIdgetTeamsMeGalleryDocumentidPixelsOriginalBytes"></a>

> Code samples

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => '*/*',
    'authorization' => 'string',
    
    );

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://app.generative.photos/api/v0/teams/me/gallery/{documentid}/pixels/original/bytes', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```python
import requests
headers = {
  'Accept': '*/*',
  'authorization': 'string'
}

r = requests.get('https://app.generative.photos/api/v0/teams/me/gallery/{documentid}/pixels/original/bytes', params={

}, headers = headers)

print r.json()

```

`GET /teams/me/gallery/{documentid}/pixels/original/bytes`

Generated variants are in PNG format.

<h3 id="getteamsmegallerydocumentidpixelsoriginalbytes-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|authorization|header|string|true|none|
|documentid|path|string|true|none|

> Example responses

> 200 Response

<h3 id="getteamsmegallerydocumentidpixelsoriginalbytes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|string|

<aside class="success">
This operation requires API key
</aside>

## Upload new original image

<a id="opIdpostTeamsMeGallery"></a>

> Code samples

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'multipart/form-data',
    'Accept' => '*/*',
    'authorization' => 'string',
    
    );

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://app.generative.photos/api/v0/teams/me/gallery', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```python
import requests
headers = {
  'Content-Type': 'multipart/form-data',
  'Accept': '*/*',
  'authorization': 'string'
}

r = requests.post('https://app.generative.photos/api/v0/teams/me/gallery', params={

}, headers = headers)

print r.json()

```

`POST /teams/me/gallery`

Please upload your images one at a time in PNG or JPEG format. You must specify non-empty filename as part of the `form-data` body.

This endpoint is synchronous. Once it returns, the `data.task.status` of the corresponding document is guaranteed to be `complete`.

> Body parameter

```yaml
object: string

```

<h3 id="postteamsmegallery-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|authorization|header|string|true|none|
|body|body|object|false|none|
|» object|body|string(binary)|true|image file|

> Example responses

> 200 Response

<h3 id="postteamsmegallery-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful|[GalleryItem](#schemagalleryitem)|

<aside class="success">
This operation requires API key
</aside>

## Create new variant of the image

<a id="opIdputTeamsMeGalleryDocumentidFacesFaceid"></a>

> Code samples

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => '*/*',
    'authorization' => 'string',
    
    );

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('PUT','https://app.generative.photos/api/v0/teams/me/gallery/{documentid}/faces/{faceid}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'authorization': 'string'
}

r = requests.put('https://app.generative.photos/api/v0/teams/me/gallery/{documentid}/faces/{faceid}', params={

}, headers = headers)

print r.json()

```

`PUT /teams/me/gallery/{documentid}/faces/{faceid}`

This endpoint operates as a "fire and forget" operation: variant-generation is queued and is processed asynchronously. You have to <a href="#opIdgetTeamsMeGalleryDocumentid">poll the status</a> of the variant after submitting the variant-generation request.

`{faceid}` path-parameter indicatates which face (in case the image has multiple faces in it) will be edited. For `{faceid}` path-parameter you can use either a numeric face index (zero-indexed, corresponding to element from `data.faces` list returned by <a href="#opIdgetTeamsMeGalleryDocumentid">document status endpoint</a>) or `default` (which is equivalent to `0`'th face index).

The "direction" of the edit is controlled by the `move`-parameter inside the request's body.

The "magnitude" of the directional edit is controlled by the `param` float optional parameter inside the requests body and is set default to 1.0

This endpoint is idempotent: `move`ing the same face in the same image in the same direction with the same magnitude will return the same variant with the same `id`.

> Body parameter

```json
//Example 1
{
  "move": "FemaleAsian"
}

//Example 2
{
  "move": "smile",
  "param": -0.6
}
```

<h3 id="putteamsmegallerydocumentidfacesfaceid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|authorization|header|string|true|none|
|documentid|path|string|true|none|
|faceid|path|string|true|none|
|body|body|[Change](#schemachange)|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|faceid|default|

> Example responses

> 200 Response

<h3 id="putteamsmegallerydocumentidfacesfaceid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GalleryItem](#schemagalleryitem)|

<aside class="success">
This operation requires API key
</aside>

# Schemas

<h2 id="tocSerror">error</h2>

<a id="schemaerror"></a>

```json
{
  "code": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|false|none|none|

<h2 id="tocStask">task</h2>

<a id="schematask"></a>

```json
{
  "status": "string",
  "error": {
    "code": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|status|string|false|none|none|
|error|[error](#schemaerror)|false|none|none|

<h2 id="tocSdata">data</h2>

<a id="schemadata"></a>

```json
{
  "pixels": "string",
  "task": {
    "status": "string",
    "error": {
      "code": "string"
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|pixels|string|false|none|none|
|task|[task](#schematask)|false|none|none|

<h2 id="tocSgalleryitem">GalleryItem</h2>

<a id="schemagalleryitem"></a>

```json
{
  "id": "string",
  "data": {
    "pixels": "string",
    "task": {
      "status": "string",
      "error": {
        "code": "string"
      }
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|Team-unique ID|
|data|[data](#schemadata)|false|none|none|

<h2 id="tocSchange">Change</h2>

<a id="schemachange"></a>

```json
//Ex. 1
{
  "move": "FemaleAsian"
}

//Ex. 2
{
  "move": "smile",
  "param": -0.6
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|move|string|true|none|none|
|param|float|false|none|defaults to 1.0|

#### Enumerated Values

|Property|Value|
|---|---|
|move|FemaleAsian|
|move|FemaleBlack|
|move|FemaleHispanic|
|move|FemaleWhite|
|move|MaleAsian|
|move|MaleBlack|
|move|MaleHispanic|
|move|MaleWhite|
|move|smile|
|move|gender|
|move|age|

