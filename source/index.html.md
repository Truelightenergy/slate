---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

includes:
  - errors

search: true
---

# Introduction

The Truelight Energy API is a simple, REST API that allows clients to send
account and usage data as well as receive account pricing information.

# Authentication

All API requests require an API token in the header. The token will be provided
by the Truelight team.

```shell
curl -X GET -H "Authorization: Token token=MY_TRUELIGHT_TOKEN" -H "Cache-Control:
no-cache" "https://truelight.com/api/accounts"
```

> Make sure to replace `MY_TRUELIGHT_TOKEN` with your API token.

Truelight expects for the API key to be included in all API requests to the
server in a header that looks like the following:

`Authorization: Token token=MY_TRUELIGHT_TOKEN`

<aside class="notice">
You must replace <code>MY_TRUELIGHT_TOKEN</code> with your personal API token.
</aside>

# Accounts

## Create an account

```shell
curl -X POST "https://truelight.com/api/accounts"
  --header "Authorization: Token token=MY_TRUELIGHT_TOKEN"
  --header "Content-Type: application/json" -d '{ "account": { "number":
"1234", "location": "11 Beacon St.", "state":
"MA", "load_zone": "SEMASS", "utility": "MECO", "load_profile": "G-3",
"voltage": "Primary", "rate_class": "R-1", "capacity_tag_kw": "60000",
"transmission_tag_kw": "100000" } }'
```

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": 1,
    "status": "usageless"
  }
}
```

This endpoint creates an account

### HTTP Request

`POST https://truelight.com/api/accounts`

### Parameters

Parameter | Required | Description
--------- | ------- | -----------
number | true | The account number
location | true | The address lines 1 & 2 for the account
state | true | The account geographic state's 2 digit abbreviation
load_zone | true | The account's load zone
utility | true | The account's utility
load_profile | true | The account's load profile
voltage | true | The account's voltage
rate_class | true | The account's rate class
capacity_tag_kw | false | The account's capacity tag in kWh
transmission_tag_kw | false | The account's transmission tag in kWh

<aside class="success">
A successful POST will return an HTTP 201
</aside>
