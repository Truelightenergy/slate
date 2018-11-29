# Accounts

## Create an account

```shell
curl -X POST "http://truelight.herokuapp.com/api/accounts"
  --header "Authorization: Token token=MY_TRUELIGHT_TOKEN"
  --header "Content-Type: application/json" -d '{ "account": { "number":
"1234", "location": "11 Beacon St.", "state":
"MA", "load_zone": "SEMASS", "utility": "MECO", "load_profile": "G-3",
"voltage": "Primary", "rate_class": "R-1", "capacity_tag_kw": "60000",
"transmission_tag_kw": "100000", summary_usage_attributes: [{ "starts_on":
"2018-10-1", "ends_on": "2018-10-31", "volume_kwh": "925.24"}] } }'
```

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": 1,
    "status": "usageless",
    "rt_lmp_price": "N/A"
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

`POST http://truelight.herokuapp.com/api/accounts`

### Parameters

| Parameter                | Required | Default | Description                                         |
| ------------------------ | -------  | ------- | --------------------------------------------------- |
| number                   | true     | N/A     | The account number                                  |
| location                 | true     | N/A     | The address lines 1 & 2 for the account             |
| state                    | true     | N/A     | The account geographic state's 2 digit abbreviation |
| load_zone                | true     | N/A     | The account's load zone                             |
| utility                  | true     | N/A     | The account's utility                               |
| load_profile             | true     | N/A     | The account's load profile                          |
| voltage                  | true     | N/A     | The account's voltage                               |
| rate_class               | true     | N/A     | The account's rate class                            |
| summary_usage_attributes | true     | N/A     | An array of summary usage parameters (more below)   |
| capacity_tag_kw          | false    | 0       | The account's capacity tag in kWh                   |
| transmission_tag_kw      | false    | 0       | The account's transmission tag in kWh               |

### Summary Usage Parameters

| Parameter  | Required | Description                                       |
| ---------- | -------- | ------------------------------------------------- |
| starts_on  | true     | The date the usage starts on in format "YYYY-M-D" |
| ends_on    | true     | The date the usage ends on in format "YYYY-M-D"   |
| volume_kwh | true     | The volume in kWh during the usage period         |

<aside class="success">
A successful POST will return an HTTP 201
</aside>

<aside class="warning">
An unsuccessful POST will return an HTTP 422 and validation error
</aside>

## Get a specific account

This endpoint returns an individual account

```shell
curl "http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>"
  -H "Authorization: Token token=MY_TRUELIGHT_TOKEN"
```

> Make sure you replace `MY_TRUELIGHT_TOKEN` with your API token

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": 1,
    "status": "intervalized",
    "rt_lmp_price": "28.8792"
  }
}
```

### HTTP Request

`GET http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>`

### URL Parameters

| Parameter  | Description                       |
| ---------- | --------------------------------- |
| ACCOUNT_ID | The ID of the Account to retrieve |

### Response Parameters

 | Parameter    | Description                                                      |
 | ------------ | ---------------------------------------------------------------- |
 | id           | The ID of the account                                            |
 | status       | The intervalization status of the account (more below)           |
 | rt_lmp_price | The RT LMP price per mWh for the account - if available - or N/A |

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

## Update a specific account

This endpoint updates an individual account

<aside class="warning">
Updating an account with new usage data erases all existing usage data
</aside>

```shell
curl -X PATCH "http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>"
  --header "Authorization: Token token=MY_TRUELIGHT_TOKEN"
  --header "Content-Type: application/json" -d '{ "account": {
summary_usage_attributes: [{ "starts_on": "2018-9-1", "ends_on": "2018-9-31",
"volume_kwh": "914.87" }, { "starts_on": "2018-10-1", "ends_on": "2018-10-31",
"volume_kwh": "925.24"}] } }'
```

> Make sure you replace `MY_TRUELIGHT_TOKEN` with your API token

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": 1,
    "status": "intervalizing",
    "rt_lmp_price": "N/A"
  }
}
```

### HTTP Request

`PATCH http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>`

### URL Parameters

| Parameter                | Required | Default | Description                                         |
| ------------------------ | -------- | ------- | --------------------------------------------------- |
| number                   | true     | N/A     | The account number                                  |
| location                 | true     | N/A     | The address lines 1 & 2 for the account             |
| state                    | true     | N/A     | The account geographic state's 2 digit abbreviation |
| load_zone                | true     | N/A     | The account's load zone                             |
| utility                  | true     | N/A     | The account's utility                               |
| load_profile             | true     | N/A     | The account's load profile                          |
| voltage                  | true     | N/A     | The account's voltage                               |
| rate_class               | true     | N/A     | The account's rate class                            |
| summary_usage_attributes | true     | N/A     | An array of summary usage parameters (more below)   |
| capacity_tag_kw          | false    | 0       | The account's capacity tag in kWh                   |
| transmission_tag_kw      | false    | 0       | The account's transmission tag in kWh               |

### Summary Usage Parameters

Parameter | Required | Description
--------- | ------- | -----------
starts_on | true | The date the usage starts on in format "YYYY-M-D"
ends_on | true | The date the usage ends on in format "YYYY-M-D"
volume_kwh | true | The volume in kWh during the usage period

### Response Parameters

| Parameter    | Description                                                      |
| ------------ | ---------------------------------------------------------------- |
| id           | The ID of the account                                            |
| status       | The intervalization status of the account (more below)           |
| rt_lmp_price | The RT LMP price per mWh for the account - if available - or N/A |

<aside class="success">
A successful PATCH will return an HTTP 200
</aside>

<aside class="warning">
An unsuccessful PATCH will return an HTTP 422 and validation error
</aside>
