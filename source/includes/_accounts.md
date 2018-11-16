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

> A validation failure with return an error payload

```json
{
  "load_profile": ["is not included in the utility's load profiles"]
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

<aside class="warning">
An unsuccessful POST will return an HTTP 422 and validation error
</aside>

## Get a specific account

This endpoint returns an individual account

```shell
curl "https://truelight.com/api/accounts/<ACCOUNT_ID>"
  -H "Authorization: Token token=MY_TRUELIGHT_TOKEN"
```

> Make sure you replace `MY_TRUELIGHT_TOKEN` with your API token

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": 1,
    "status": "intervalized"
  }
}
```

### HTTP Request

`GET https://truelight.com/api/accounts/<ACCOUNT_ID>`

### URL Parameters

 Parameter | Description
---------- | -----------
| ACCOUNT_ID | The ID of the Account to retrieve |

### Response Parameters

 Parameter | Description
---------- | -----------
id | The ID of the account
status | The intervalization status of the account (more below)
rt_lmp_price | The RT LMP price for the account, if available, or N/A

### Account status
The account's intervalization status can be one of "usageless", "intervalizing",
"intervalized", or "intervalization_error".

* If we have no usage data the status is
"usageless".
* If we're currently creating interval usage data in the background the
status is "intervalizing".
* If we have interval usage data the status is
"intervalized".
* Finally, if there was an error generating the interval data the
status is "intervalization_error".

<aside class="success">
A successful GET will return an HTTP 200
</aside>

<aside class="warning">
An unsuccessful GET will return an HTTP 404
</aside>
