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

* email generation
* Dialer lead generation
* Storage of leads, application journeys in Suite CRM
* Some custom functions for specific countries/verticals


## Salesforce Marketing Cloud (Email)

### Top 3 result email that includes the top three results that customer sees on the result page can be sent after customers fill out the email and reach the result page

### Required Query Parameters for Initial Event

Parameter | Description
--------- | -----------
email | Salesforce Marketing Cloud cannot function without an email address
vertical | Used to identify segmentation config
locale | Used to identify segmentation config
language | Used to identify which email language to be sent
attributes/usercityEventName | The first event with the listed required parameters must have the usercityEventName for Salesforce Markeitng Cloud to identify which type of email to send
source_url | Used to indicate where the user dropped off on their user journey in order for them to continue their journey
dm_consent | has to be "true" for Salesforce Marketing Cloud to send email
tc_consent | has to be "true" for Salesforce Marketing Cloud to send email
attributes/top products | all product information for each of the top three product should be included (see example)


### Abandon email can be sent after customers to resume application (step one of the application), after customer submit the first step of application form and leave the application form without finishing it.

### Required Query Parameters for Initial Event

Parameter | Description
--------- | -----------
email | Salesforce Marketing Cloud cannot function without an email address
vertical | Used to identify segmentation config
locale | Used to identify segmentation config
language | Used to identify which email language to be sent
attributes/usercityEventName | The first event with the listed required parameters must have the usercityEventName for Salesforce Markeitng Cloud to identify which type of email to send
source_url | Used to indicate where the user dropped off on their user journey in order for them to continue their journey
dm_consent | has to be "true" for Salesforce Marketing Cloud to send email
tc_consent | has to be "true" for Salesforce Marketing Cloud to send email
attributes/application_form/completed |  has to be "false" for Salesforce Marketing Cloud to send abandon email

### Optional Parameters
the payload should include all variables for the email template.
e.g. travel insurance abandon cart email, include related fields from the funnel and first step of the application
attributes/application form/provider | variable to display in email template
attributes/application form/destinations | variable to display in email template


### Order Confirmation email can be sent after a customer completes an application form to notify them that the application is already been received.

### Required Query Parameters for Initial Event

Parameter | Description
--------- | -----------
email | Salesforce Marketing Cloud cannot function without an email address
vertical | Used to identify segmentation config
locale | Used to identify segmentation config
language | Used to identify which email language to be sent
attributes/usercityEventName | The first event with the listed required parameters must have the usercityEventName for Salesforce Markeitng Cloud to identify which type of email to send
source_url | Used to indicate where the user dropped off on their user journey in order for them to continue their journey
dm_consent | has to be "true" for Salesforce Marketing Cloud to send email
tc_consent | has to be "true" for Salesforce Marketing Cloud to send email
attributes/application_form/completed |  has to be "true" for Salesforce Marketing Cloud to send order confirmation email

### Optional Parameters
the payload should include all variables for the email template.
e.g. travel insurance confirmation email
Parameter | Description
--------- | -----------
attributes/application form/ name | variable to display in email template
attributes/application form/policyNumber | variable to display in email template
attributes/application form/provider | variable to display in email template
attributes/application form/category | variable to display in email template
attributes/application form/date/start | variable to display in email template
attributes/application form/date/end | variable to display in email template
attributes/application form/destinations | variable to display in email template
attributes/application form/phone | variable to display in email template

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
