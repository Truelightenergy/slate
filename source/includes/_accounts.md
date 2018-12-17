# Accounts

## Create an account

```shell
curl -X POST "http://truelight.herokuapp.com/api/accounts"
  --header "Authorization: Token token=MY_TRUELIGHT_TOKEN"
  --header "Content-Type: application/json" -d '{ "account": { "account_number":
"1234", "location": "11 Beacon St.", "city": "Philadelphia", "state":
"PA", "load_zone": "APS-PA", "utility": "WPP", "load_profile": "GPC",
"voltage": "Primary", "rate_class": "10", "capacity_tag_kw": "60000",
"transmission_tag_kw": "100000", summary_usage_attributes: [{ "starts_on":
"2018-10-1", "ends_on": "2018-10-31", "volume_kwh": "925.24"}] } }'
```

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": 1,
    "intervalization_status": "usageless",
    "rt_lmp_price": null
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
| account_number           | true     | N/A     | The account number                                  |
| location                 | true     | N/A     | The address lines 1 & 2 for the account             |
| state                    | true     | N/A     | The account geographic state's 2 digit abbreviation |
| load_zone                | true     | N/A     | The account's load zone                             |
| utility                  | true     | N/A     | The account's utility                               |
| load_profile             | true     | N/A     | The account's load profile                          |
| voltage                  | true     | N/A     | The account's voltage                               |
| rate_class               | true     | N/A     | The account's rate class                            |
| summary_usage_attributes | false    | N/A     | An array of summary usage parameters (more below)   |
| capacity_tag_kw          | false    | 0       | The account's capacity tag in kWh                   |
| transmission_tag_kw      | false    | 0       | The account's transmission tag in kWh               |
| flow_start               | false    | N/A     | The account's flow start date (YYYY-M-D)            |
| flow_end                 | false    | N/A     | The account's flow end date (YYYY-M-D)              |

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
    "intervalization_status": "intervalized",
    "rt_lmp_price": "28.8792",
    "flow_start": null,
    "flow_end": null,
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

| Parameter                    | Description                                                       |
| ---------------------------- | ----------------------------------------------------------------- |
| id                           | The ID of the account                                             |
| intervalization_status       | The intervalization status of the account (more below)            |
| rt_lmp_price                 | The RT LMP prices the account                                     |
| flow_start                   | The account's flow start date                                     |
| flow_end                     | The account's flow end date                                       |

### Account intervalization status
The account's intervalization status can be one of "usageless", "intervalizing",
"intervalized", or "intervalization_error".

* If we have no usage data the intervalization status is
"usageless".
* If we're currently creating interval usage data in the background the
intervalization status is "intervalizing".
* If we have interval usage data the intervalization status is
"intervalized".
* Finally, if there was an error generating the interval data the
intervalization status is "intervalization_error".

<aside class="success">
A successful GET will return an HTTP 200
</aside>

<aside class="warning">
An unsuccessful GET will return an HTTP 404
</aside>

## Update a specific account

This endpoint updates an individual account

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
    "intervalization_status": "intervalizing",
    "rt_lmp_price": null,
    "flow_start": null,
    "flow_end": null,
  }
}
```

### HTTP Request

`PATCH http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>`

### URL Parameters

| Parameter                | Required | Default | Description                                         |
| ------------------------ | -------- | ------- | --------------------------------------------------- |
| account_number           | true     | N/A     | The account number                                  |
| location                 | true     | N/A     | The address lines 1 & 2 for the account             |
| state                    | true     | N/A     | The account geographic state's 2 digit abbreviation |
| load_zone                | true     | N/A     | The account's load zone                             |
| utility                  | true     | N/A     | The account's utility                               |
| load_profile             | true     | N/A     | The account's load profile                          |
| voltage                  | true     | N/A     | The account's voltage                               |
| rate_class               | true     | N/A     | The account's rate class                            |
| summary_usage_attributes | false    | N/A     | An array of summary usage parameters (more below)   |
| capacity_tag_kw          | false    | 0       | The account's capacity tag in kWh                   |
| transmission_tag_kw      | false    | 0       | The account's transmission tag in kWh               |
| flow_start               | false    | N/A     | The account's flow start date (YYYY-M-D)            |
| flow_end                 | false    | N/A     | The account's flow end date (YYYY-M-D)              |

### Summary Usage Parameters

Parameter | Required | Description
--------- | ------- | -----------
starts_on | true | The date the usage starts on in format "YYYY-M-D"
ends_on | true | The date the usage ends on in format "YYYY-M-D"
volume_kwh | true | The volume in kWh during the usage period

### Response Parameters

| Parameter                    | Description                                                      |
| ---------------------------- | ---------------------------------------------------------------- |
| id                           | The ID of the account                                            |
| intervalization_status       | The intervalization status of the account (more below)           |
| rt_lmp_price                 | The RT LMP prices the account                                     |
| flow_start                   | The account's flow start date                                     |
| flow_end                     | The account's flow end date                                       |

<aside class="success">
A successful PATCH will return an HTTP 200
</aside>

<aside class="warning">
An unsuccessful PATCH will return an HTTP 422 and validation error
</aside>

## De-activate an account

This endpoint de-activates an account

```shell
curl -X PATCH "http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>"
  --header "Authorization: Token token=MY_TRUELIGHT_TOKEN"
  --header "Content-Type: application/json" -d '{ "account": {
status: "inactive" } }'
```

> Make sure you replace `MY_TRUELIGHT_TOKEN` with your API token

### HTTP Request

`PATCH http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>`

### URL Parameters

| Parameter                | Required | Default  | Description                                         |
| ------------------------ | -------- | -------- | --------------------------------------------------- |
| status                   | false    | "active" | The account status ("active" or "inactive")         |

<aside class="success">
A successful PATCH will return an HTTP 200
</aside>

<aside class="warning">
An unsuccessful PATCH will return an HTTP 422 and validation error
</aside>
