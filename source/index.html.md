---
title: SIMP2

language_tabs:

- shell
- php

search: false
code_clipboard: true
---

# Introduction

Welcome to the SIMP2 Documentation

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://staging.bytelog.io/api/v1/events" \
  -H "X-API-KEY: example_api_key"
```

> Make sure to replace `example_api_key` with your API key.

SIMP2 uses API keys to allow access to the API. The api_key is provided to you by us.

The api key is expected to be included in all API requests in a header that looks like the
following:

`X-API-KEY: example_api_key`

# Company transaction tokens

> To query for a company, use this header:

```shell
# With shell, you can just pass the correct header with each request
curl "https://staging.bytelog.io/api/v1/events" \
  -H "X-API-KEY: example_api_key" \
  -H "company-transaction-token: example_ctt"
```

> Make sure to replace `example_ctt` with a ctt that you want to represent.

SIMP2 uses a token to identify what company is querying when you are using a payment method's api-key.


`company-transaction-token: example_ctt`


# Events

## Get all events

```shell
curl "https://staging.bytelog.io/api/v1/events" \
  -H "X-API-KEY: example_api_key"
```

> The above command returns JSON structured like this:

```json
{
  "count": 2,
  "events": [
    {
      "id": 1,
      "name": "click",
      "trace_id": "03775690",
      "date": "2021-08-15T23:41:41.507Z",
      "data": {
        "clicked_on": "sign up button"
      }
    },
    {
      "id": 5,
      "name": "paypal_payment",
      "trace_id": "03758690",
      "date": "2021-08-15T23:43:41.507Z",
      "data": {
        "client_id": "17039485"
      }
    }
  ]
}
```

This endpoint retrieves all events.

### HTTP Request

`GET https://staging.bytelog.io/api/v1/events`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
start | false | If included, the result will contain events after the start date. Format AAAA-MM-DD.
end | false | If included, the result will contain events prior the end date. Format AAAA-MM-DD.

## Register an event

```shell
curl "https://staging.bytelog.io/api/v1/events" \
  -X POST \
  -H "X-API-KEY: example_api_key" \
  -H "Content-Type: application/json" \
  -d '{"id": 1,"name": "click","trace_id": "03775690","date": "2021-07-04 13:34:56","data": {"clicked_on": "sign up button"}'
```

> The above command returns 201 Created with and empty body.

> The above command returns 400 if an invalid body is provided.

```json
{
  "error": "validation error"
}
```

This endpoint saves a new event.

### HTTP Request

`POST https://staging.bytelog.io/api/v1/events`

### Body Example

<code lang="json" >
  {
    "id": 5,
    "name": "paypal_payment",
    "trace_id": "03758690",
    "data": {
      "client_id": "17039485"
    }
  }
</code>

### Body Schema

Parameter | Description
--------- | -----------
id        | The Event id
name      | A event descriptor or friendly name
trace_id  | An id to identify a flow of events or events that are connected
data      | Any json data object you need

## Follow trace id

```shell
curl "https://staging.bytelog.io/api/v1/events/trace/:trace_id" \
  -H "X-API-KEY: example_api_key"
```

> The above command returns JSON structured like this:

```json
{
  "count": 2,
  "events": [
    {
      "id": 1,
      "name": "click",
      "trace_id": "03775690",
      "date": "2021-08-15T23:45:41.507Z",
      "data": {
        "clicked_on": "sign up button"
      }
    },
    {
      "id": 1,
      "name": "click",
      "trace_id": "03775690",
      "date": "2021-08-15T23:45:45.507Z",
      "data": {
        "clicked_on": "cancel sign up"
      }
    }
  ]
}
```

With this endpoint you can get related events or follow users path throughout your business logic

### HTTP Request

`GET https://staging.bytelog.io/api/v1/events/trace/:trace_id`

### Route parameter

Parameter | Description
--------- | -----------
trace_id  | The trace_id you want to follow.