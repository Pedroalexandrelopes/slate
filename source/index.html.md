---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

ilove.me Bot


# Authentication

> To authorize, use this code:

```javascript
const botApi = require('botAPI');

let api = botApi.authorize('token');
```

> Make sure to replace `token` with your Token.



BotAPI uses token to allow access to the API.

BotAPI expects for the Token to be included in all API requests to the server in a header that looks like the following:

`Authorization: token`

<aside class="notice">
You must replace <code>token</code> with the token provided in the chat bot.
</aside>





# Webview


## Get User Details

```javascript
const botApi = require('botAPI');
let api = botApi.authorize('token');
let user = api.user.get();
```

> The above command returns JSON structured like this:

```json
{
  "username": "Jorge",
  "userphone": "9999999999",
  "business": {
    "id": 1,
    "name": "Salão São Paulo 1",
    "locale": "pt_BR",
    "culture": "pt-br"
  },
  "suggestions": [
    {
      "id": 1,
      "name": "Massagem relaxante"
    },
    {
      "id": 2,
      "name": "Limpeza de pele"
    },
  ],
  "recent": [
    {
      "id": 145,
      "name": "Manicure",
      "price": 49.9,
      "category": "Mão"
    },
  ]
}
```

This endpoint retrieves detail from the chat token.

### HTTP Request

`GET http://example.com/api/user/details`










## Search Services


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.services.get('text');
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "id": 1,
      "name": "Massagem relaxante"
    },
    {
      "id": 2,
      "name": "Limpeza de pele"
    },
  ],
  "categories": [
    {
      "id": 1,
      "name": "Manicure"
    },
  ],
  "services": [
    {
      "id": 145,
      "name": "Manicure especial",
      "price": 49.9,
      "category": "Mão"
    },
    {
      "id": 146,
      "name": "Manicure simples",
      "price": 32,
      "category": "Mão"
    },
    {
      "id": 147,
      "name": "Massagem simples",
      "price": 59,
      "category": "Massagem"
    },
  ]
}
```

This endpoint search for service suggestions and categories.


### HTTP Request

`GET http://example.com/api/services/term`

`GET http://example.com/api/services/<ID>`


### URL Parameters

Parameter | Description
--------- | -----------
term | The text to search
ID | The ID of the service category












## Get Availabilities


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.availabilities.get('2018-11-28');
```

> The above command returns JSON structured like this:

```json
{
  "date": "2018-11-28",
  "availabilities": [
    {
      "id": 1,
      "time": "9:30",
      "price": 35.50,
      "professionals": [
        {
          "id": 1,
          "name": "António",
        },
        {
          "id": 2,
          "name": "Maria",
        }
      ],
    },
    {
      "id": 2,
      "time": "10:30",
      "price": 35.50,
      "professionals": [
        {
          "id": 1,
          "name": "António",
        },
      ],
    },
  ],
}
```

This endpoint retrieves the availabilities.


### HTTP Request

`GET http://example.com/api/service/<ID>/availabilities/<date>`

### URL Parameters

Parameter | Description
--------- | -----------
date | The date selected (yyyy-MM-dd)
ID | service id








## Availability Lock


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.lock.post(1, 2);
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "slot": {
    "id": 12,
  }
}
```

This endpoint locks the selected availability slot.


### HTTP Request

`POST http://example.com/api/service/<ID>/availabilities/<aID>/lock`

### URL Parameters

Parameter | Description
--------- | -----------
ID | Id of the selected service
aID | Id of the availability







## Availability Release


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.release.post(12);
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
}
```

This endpoint releases the locked availability slot.


### HTTP Request

`POST http://example.com/api/service/<ID>/availabilities/<aID>/release`

### URL Parameters

Parameter | Description
--------- | -----------
ID | Id of the selected service
aID | Id of the availability








## Availability Confirm


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.confirmBooking.post(12);
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
}
```

This endpoint retrieves the availabilities.


### HTTP Request

`POST http://example.com/api/service/<ID>/availabilities/<aID>/confirm`

### URL Parameters

Parameter | Description
--------- | -----------
ID | Id of the selected service
aID | Id of the availability










## Set User Phone (SMS Code)


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.smscode.post('999999999');
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
}
```

This endpoint sends the user phone and send an SMS with a code.


### HTTP Request

`POST http://example.com/api/user/phone/<phone>`

### URL Parameters

Parameter | Description
--------- | -----------
phone | The user input phone







## Confirm User Phone


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.confirmPhone.post('123456');
```

> The above command returns JSON structured like this:

```json
{
  "success": true,
}
```

This endpoint confirms the user phone.


### HTTP Request

`POST http://example.com/api/user/phone/confirm/<code>`

### URL Parameters

Parameter | Description
--------- | -----------
code | The sent code











## Get User Bookings


```javascript
const botAPI = require('botAPI');

let api = botAPI.authorize('token');
let services = api.bookings.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "date": "2018-11-28",
    "time": "9:30",
    "state": "booked",
    "service": {
      "id": 145,
      "name": "Manicure",
      "price": 49.9
    },
    "professional": {
      "id": 145,
      "name": "António"
    }
  },
]
```

This endpoint retrieves the user past bookings


### HTTP Request

`GET http://example.com/api/user/bookings`







# Chat




## Get Profile


```javascript
const botAPI = require('botAPI');

let profile = api.profile.get(1, 2);
```

> The above command returns JSON structured like this:

```json
{
  "businessToken": "token",
  "userName": "Joaquim",
  "userPhone": "999999999",
}
```

This endpoint retrieves the user profile details


### HTTP Request

`GET http://example.com/profile/<bID><cID>`

### URL Parameters

Parameter | Description
--------- | -----------
bID | The ID of the business facebook page
cID | The ID of the user chat-business








## Save Profile


```javascript
const botAPI = require('botAPI');

let profile = api.profile.save(1, 2, 'Joaquim' '{"id": 1}');
```

> The above command returns JSON structured like this:

```json
{
  "success": true
}
```

This endpoint saves the user profile details


### HTTP Request

`POST http://example.com/profile/<bID><cID><username><businesses>`

### URL Parameters

Parameter | Description
--------- | -----------
bID | The ID of the business facebook page
cID | The ID of the user chat-business
username | Name of the user
businesses | Ids of the other pages the user is registered with











## Save Message Event


```javascript
const botAPI = require('botAPI');

let profile = api.message.save(1, 2, 'message', 'seqID', 123456);
```

> The above command returns JSON structured like this:

```json
{
  "success": true
}
```

This endpoint saves the user message event


### HTTP Request

`POST http://example.com/message/<bID><cID><message><seqID><timestamp>`

### URL Parameters

Parameter | Description
--------- | -----------
bID | The ID of the business facebook page
cID | The ID of the user chat-business
message | Message sent by the user
seqID | Facebook message sequence ID
timestamp | Facebook message timestamp