---
title: RESTful API
---

Before you can start calling the server APIs you need to take care of the following steps first:

1. [Create a free account on statsig.com](#step1)
2. Get an API key from the admin console
3. Issue API requests

<a name="step1"></a>

#### Step 1 - Create a free account on [statsig.com](https://console.statsig.com/sign_up)

An account will let you use the Statsig Console, where you can manage all of
your Feature Gates and Dynamic Configs. Note that you will be able to invite
others to your Statsig Projects, so they can also interact with your gates and
configs.

<a name="step2"></a>

#### Step 2 - Get an API key from [statsig.com](https://console.statsig.com/)

An API key is required in every API request. There are two different types of API keys:

- **Server-side secret Key** which should only be used from a secure server and never shipped on clients.
- **Client-SDK Key** which can be embedded inside mobile apps and front-end client web apps.

If you are in doubt, use the Client-SDK key.

<a name="step3"></a>

#### Step 3 - Issue API request

Our API is built on top of HTTPS. You can authenticate via header
`statsig-api-key`. All of our APIs use method `POST`, and you can set parameters
by using a JSON object as the request data.

##### Check a Feature Gate

```bash
curl \
  --header "statsig-api-key: <YOUR-SDK-KEY>" \
  --header "Content-Type: application/json" \
  --request POST \
  --data '{"user": { "userID": "42" },"gateName":"<YOUR-GATE-NAME>"}' \
  "https://api.statsig.com/v1/check_gate"
```

Response:
`{"name":"YOUR-GATE-NAME","value":false}`

##### Get a Dynamic Config value

```bash
curl \
  --header "statsig-api-key: <YOUR-SDK-KEY>" \
  --header "Content-Type: application/json" \
  --request POST \
  --data '{"user": { "userID": "42" },"configName":"<YOUR-CONFIG-NAME>"}' \
  "https://api.statsig.com/v1/get_config"
```

Response:
`{"name":"YOUR-CONFIG-NAME","value":{"a":1,"b":2},"group":"default"}`

##### Log an event

```bash
curl \
  --header "statsig-api-key: <YOUR-SDK-KEY>" \
  --header "Content-Type: application/json" \
  --request POST \
  --data '{"events": [{"user": { "userID": "42" }, "time": 1616826986211, "eventName": "test_api_event"}]}' \
  "https://api.statsig.com/v1/log_event"
```

Response:
`{"success":true}`