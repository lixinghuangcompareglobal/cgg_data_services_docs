---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python
  - shell


toc_footers:

includes:
  - errors

search: true
---

# Introduction

Test: Welcome to our internal API documentation. This documentation is intended for both business and developer users to understand what API functionality we support and how to use it.

Currently, we have documented two API groups:

1. Events API - which interacts with Usercities
2. Data services API - which interacts with our Data warehouse

# Events API

The Events API requires no authentication. It provides a single entry point that captures events from various stages of the user journey. Here is a summary of the functionality that it currently provides:

* 
email generation
* Salesforce email generation
* Dialer lead generation
* Storage of leads, application journeys in Suite CRM
* Some custom functions for specific countries/verticals


## Salesforce Marketing Cloud (Email)
generate top 3 results (user reaches result page)
Salesforce will generate corresponding email depends at the time user submits their email address and land on result page.

### Required Query Parameters for Initial Event

Parameter | Description
--------- | -----------
email | email cannot function without an email address
vertical | Used to identify segmentation config
locale | Used to identify segmentation config
language | Required for some countries to send email in a specific language
dm_consent| DM consent has to be true
tc_consent| TC consent has to be true
attributes/usercityEventName | Required, need to use this to differentiate type of email sent.
attributes/top products | required for abandon card email

### Required Query Parameters for Subsequent Events

Parameter | Description
--------- | -----------
email | email cannot function without an email address
vertical | Used to identify segmentation config
locale | Used to identify segmentation config
language | Required but not Used
attributes/usercityEventName | The first event with the listed required parameters must have the usercityEventName set to 'postFromFunnel' in order to generate a TMG lead. Subsequent events can have a different type
source_url | Used to indicate where the user dropped off on their user journey


### Send Policy Documents to Customer


> To call lambda directly:

```python

import lambda_service



lambda_service.execute("{"ULR":}")


```

```java



```

> To call from API Gateway:

```python

import lambda_service



lambda_service.execute("{"ULR":}")


```

```java



```

To trigger a policy document email, the following internal API must be used:






## Dialer Lead Generation

> Example of calling the Events API to generate a Lead in TMG

```python

import httplib2

headers = {
  'Content-type': 'application/json',
  'Usercity-Auth-Token': 'c1550bd0-e187-4bce-9044-846eb89cad8f'
  }

payload = {
  "phone": "995551234",
  "email": "steve.walsh@compareglobalgroup.com",  
  "vertical": "car_insurance",
  "locale": "HK",  
  "language": "ES",
  "attributes": {
    "usercityEventName": "postFromFunnel",
  },
  "source_url": "http://moneyhero.hk"
}
events_url = "http://moneyhero.hk/api/events/submit"

http = httplib2.Http()
http.request(events_url, 'POST', body = payload, headers = headers)

```

In terms of dialers, currently, we support TMG only. The dialer lead generation functionality requires an initial event message that contains the required query parameters listed below. Subsequent events can be sent with additional required fields listed below. The dialer lead generation logic uses an email address to merge all of these events together and allow for 15 minutes of inactivity before sending the lead to the dialer.

### Required Query Parameters for Initial Event

Parameter | Description
--------- | -----------
phone | The dialer cannot function without a phone number
email | Usercity uses the email address field to determine unique users and merge all events associated with a user journey (within last 15 minutes together)
vertical | Used to identify segmentation config
locale | Used to identify segmentation config
language | Required but not Used
attributes/usercityEventName | The first event with the listed required parameters must have the usercityEventName set to 'postFromFunnel' in order to generate a TMG lead. Subsequent events can have a different type
source_url | Used to indicate where the user dropped off on their user journey

### Required Query Parameters for Subsequent Events

Parameter | Description
--------- | -----------
phone | The dialer cannot function without a phone number
email | Usercity uses the email address field to determine unique users and merge all events associated with a user journey (within last 15 minutes together)
vertical | Used to identify segmentation config
locale | Used to identify segmentation config
language | Required but not Used
attributes/usercityEventName | The first event with the listed required parameters must have the usercityEventName set to 'postFromFunnel' in order to generate a TMG lead. Subsequent events can have a different type
source_url | Used to indicate where the user dropped off on their user journey

## Suite CRM

## Custom functions

### Payment Guard - Hong Kong Travel Insurance

### Email Completed Applications to Provider - Hong Kong Personal Loan

### Application Forms submitted to MULE for Provider - Singapore


# Data Services API

## Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint retrieves a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete
