---
title: EventifyPro API Reference

language_tabs:
  - shell
  - ruby

toc_footers:
  - <a href='http://eventify.pro' target="_blank">Sign Up for an API Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# EventifyPro API

EventifyPro provides functionality for publishing events. It's always good to have data-driven systems.
To do so you should start collecting data. Events is one of the best ways to figure out what's going on in your system.

We've created this API to allow developers to publish events easily.
Currently we have [Ruby gem](https://github.com/smakagon/eventify_pro) which supports this API.
With that gem you can integrate EventifyPro into any Ruby-application.

Using this API you can easily integrate EventifyPro into your project.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request

curl "http://api.eventify.pro/v1/events"
  -H "Authorization: secret"
```

```ruby
require 'eventify_pro'

eventify_client = EventifyPro::Client.new(api_key: 'secret')
```

> Make sure to replace `secret` with your API key.

EventifyPro uses API keys to allow access to the API. Go to [EventifyPro](http://eventify.pro) to get your API Key and start publishing events.

EventifyPro expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: secret`

Don't forget to replace `secret` with your personal API key.

Response for request with invalid Api key:

Error Code | Meaning
---------- | -------
401 | Unauthorized.
    | Response: `{ "error_message": "Invalid API key" }`

# Events

## Publish Event

```shell
curl "http://api.eventify.pro/v1/events"
  -X POST -H "Authorization: secret"
  --data "type=UserSignedUp&data={\"user_id\":1,\"user_name\":\"John\"}"
```

```ruby
require 'eventify_pro'

client = EventifyPro::Client.new(api_key: 'secret')
client.publish(type: 'UserSignedUp', data: { user_id: 1, user_name: 'John' })
```

> The above command returns JSON structured like this:

```json
{
  "id":"3a6c67c3-3077-446d-81ec-ebc89d20896d",
  "type":"UserSignedUp",
  "data":"{\"user_id\":1,\"user_name\":\"John\"}",
  "created_at":"2017-06-21 14:53:35"
}
```

This endpoint creates an event

### HTTP Request

`POST http://api.eventify.pro/v1/events`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
type | true | String that should describe event that occurred in your system. ProfileCreated, OrderProcessed, etc.
data | true | It's a JSON that can include any data related to event that you need.

Error Responses:

Error Code | Meaning
---------- | -------
422 | Unprocessable Entity
    | Response: `{ "error_message": "Please, provide type and data attributes" }`
500 | Internal Server Error
    | Response: `{ "error_message": "Couldn't create event" }`

## Get All Events

```shell
curl "http://api.eventify.pro/v1/events"
  -H "Authorization: secret"
```

```ruby
# not implemented yet
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": "a98185f9-61a2-44f2-93d0-1dfd8f7a7790",
    "type": "OrderCreated",
    "data": "{\"id\":  \"1\", \"amount\": \"1500\"}",
    "created_at": "2017-06-07 18:28:59"
  },
  {
    "id": "591aaf42-f4e0-4b41-bd80-b537b6d589c7",
    "type": "OrderCancelled",
    "data": "{\"id\":  \"1\", \"cancelled_by\": \"user\"}",
    "created_at": "2017-06-07 18:49:22"
  }
]
```

This endpoint retrieves all events.

### HTTP Request

`GET http://api.eventify.pro/v1/events`

Error Responses:

Error Code | Meaning
---------- | -------
500 | Internal Server Error
    | Response: `{ "error_message": "Couldn't get events" }`

## Get a Specific Event

```shell
curl "http://api.eventify.pro/v1/events/3a6c67c3-3077-446d-81ec-ebc89d20896d"
  -H "Authorization: secret"
```

```ruby
# not implemented yet
```

> The above command returns JSON structured like this:

```json
{
  "id":"3a6c67c3-3077-446d-81ec-ebc89d20896d",
  "type":"UserSignedUp",
  "data":"{\"user_id\":1,\"user_name\":\"John\"}",
  "created_at":"2017-06-21 14:53:35"
}
```

This endpoint retrieves a specific event

### HTTP Request

`GET http://api.eventify.pro/v1/events/:event_id`

### URL Parameters

Parameter | Description
--------- | -----------
event_id | The ID of the event to retrieve

Error Responses:

Error Code | Meaning
---------- | -------
404 | Not Found
    | Response: `{ "error_message": "Coundln't find event" }`

