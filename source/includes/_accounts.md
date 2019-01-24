# Accounts

## Create an account

```shell
curl -X POST "http://truelight.herokuapp.com/api/accounts"
  --header "Authorization: Token token=MY_TRUELIGHT_TOKEN"
  --header "Content-Type: application/json" -d '{ "account": { "account_number":
"1234", "location": "11 Beacon St.", "city": "Philadelphia", "state":
"PA", "load_zone": "APS-PA", "utility": "WPP", "load_profile": "GPC",
"voltage": "Primary", "rate_class": "10", "summary_usage_attributes": [{ "starts_on":
"2018-10-1", "ends_on": "2018-10-31", "volume_kwh": "925.24"}],
"capacity_tags_attributes": [{ "start_date": "2018-6-1", "end_date": "2019-5-31",
"value_kw": "60000" }], "transmission_tags_attributes": [{ "start_date":
"2018-1-1", "end_date: "2018-12-31", "value_kw": "100000" }] } }'
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "status": "active",
  "intervalization_status": "usageless",
  "rt_lmp_price": null,
  "flow_start": null,
  "flow_end": null,
  "capacity_tags": [
    {
      "id": 1,
      "start_date": "2018-6-1",
      "end_date": "2019-5-31",
      "value_kw": 60000
    }
  ],
  "transmission_tags": [
    {
      "id": 1,
      "start_date": "2018-1-1",
      "end_date": "2018-12-31",
      "value_kw": 100000
    }
  ]
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

| Parameter                    | Required | Default | Description                                          |
| ---------------------------- | -------- | ------- | ---------------------------------------------------- |
| account_number               | true     | N/A     | The account number                                   |
| location                     | true     | N/A     | The address lines 1 & 2 for the account              |
| state                        | true     | N/A     | The account geographic state's 2 digit abbreviation  |
| load_zone                    | true     | N/A     | The account's load zone                              |
| utility                      | true     | N/A     | The account's utility                                |
| load_profile                 | true     | N/A     | The account's load profile                           |
| voltage                      | true     | N/A     | The account's voltage                                |
| rate_class                   | true     | N/A     | The account's rate class                             |
| summary_usage_attributes     | false    | N/A     | An array of summary usage parameters (more below)    |
| capacity_tags_attributes     | true     | N/A     | An array of capacity tag parameters (more below)     |
| transmission_tags_attributes | true     | N/A     | An array of transmission tag parameters (more below) |
| flow_start                   | false    | N/A     | The account's flow start date (YYYY-M-D)             |
| flow_end                     | false    | N/A     | The account's flow end date (YYYY-M-D)               |

### Summary Usage Parameters

| Parameter  | Required | Description                                       |
| ---------- | -------- | ------------------------------------------------- |
| starts_on  | true     | The date the usage starts on in format "YYYY-M-D" |
| ends_on    | true     | The date the usage ends on in format "YYYY-M-D"   |
| volume_kwh | true     | The volume in kWh during the usage period         |

### Capacity Tag Parameters

| Parameter  | Required | Description                                     |
| ---------- | -------- | ----------------------------------------------- |
| start_date | true     | The date the tag starts on in format "YYYY-M-D" |
| end_date   | true     | The date the tag ends on in format "YYYY-M-D"   |
| value_kw   | true     | The value of the tag in kw                      |

### Transmission Tag Parameters

| Parameter  | Required | Description                                     |
| ---------- | -------- | ----------------------------------------------- |
| start_date | true     | The date the tag starts on in format "YYYY-M-D" |
| end_date   | true     | The date the tag ends on in format "YYYY-M-D"   |
| value_kw   | true     | The value of the tag in kw                      |

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
  "id": 1,
  "status": "active",
  "intervalization_status": "intervalized",
  "rt_lmp_price": {
    "id": 10,
    "ancillaries_price": 1.7129,
    "capacity_price": 0.1628,
    "starts_on": "2018-12-01",
    "ends_on": "2018-12-31",
    "lmp_price": 36.6938,
    "losses_price": 2.13445704,
    "rps_price": 0.261,
    "total_price_per_mwh": 41.4976248,
    "transmission_ancillaries_price": -0.0086,
    "transmission_price": 0.1653,
  },
  "flow_start": null,
  "flow_end": null,
  "capacity_tags": [
    {
      "id": 1,
      "start_date": "2018-6-1",
      "end_date": "2019-5-31",
      "value_kw": 60000
    }
  ],
  "transmission_tags": [
    {
      "id": 1,
      "start_date": "2018-1-1",
      "end_date": "2018-12-31",
      "value_kw": 100000
    }
  ]
}
```

### HTTP Request

`GET http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>`

### URL Parameters

| Parameter  | Description                       |
| ---------- | --------------------------------- |
| ACCOUNT_ID | The ID of the Account to retrieve |

### Response Parameters

| Parameter                    | Description                                            |
| ---------------------------- | ------------------------------------------------------ |
| id                           | The ID of the account                                  |
| status                       | The account status ("active" or "inactive")            |
| intervalization_status       | The intervalization status of the account (more below) |
| rt_lmp_price                 | The RT LMP prices the account                          |
| flow_start                   | The account's flow start date                          |
| flow_end                     | The account's flow end date                            |
| capacity_tags                | An array of the account's capacity tags                |
| transmission_tags            | An array of the account's transmission tags            |

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
  "id": 1,
  "status": "active",
  "intervalization_status": "intervalizing",
  "rt_lmp_price": null,
  "flow_start": null,
  "flow_end": null,
  "capacity_tags": [
    {
      "id": 1,
      "start_date": "2018-6-1",
      "end_date": "2019-5-31",
      "value_kw": 60000
    }
  ],
  "transmission_tags": [
    {
      "id": 1,
      "start_date": "2018-1-1",
      "end_date": "2018-12-31",
      "value_kw": 100000
    }
  ]
}
```

### HTTP Request

`PATCH http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>`

### Parameters

| Parameter                    | Required | Default | Description                                          |
| ---------------------------- | -------- | ------- | ---------------------------------------------------- |
| account_number               | false    | N/A     | The account number                                   |
| location                     | false    | N/A     | The address lines 1 & 2 for the account              |
| state                        | false    | N/A     | The account geographic state's 2 digit abbreviation  |
| load_zone                    | false    | N/A     | The account's load zone                              |
| utility                      | false    | N/A     | The account's utility                                |
| load_profile                 | false    | N/A     | The account's load profile                           |
| voltage                      | false    | N/A     | The account's voltage                                |
| rate_class                   | false    | N/A     | The account's rate class                             |
| summary_usage_attributes     | false    | N/A     | An array of summary usage parameters (more below)    |
| capacity_tags_attributes     | false    | N/A     | An array of capacity tag parameters (more below)     |
| transmission_tags_attributes | false    | N/A     | An array of transmission tag parameters (more below) |
| flow_start                   | false    | N/A     | The account's flow start date (YYYY-M-D)             |
| flow_end                     | false    | N/A     | The account's flow end date (YYYY-M-D)               |

### Summary Usage Parameters

| Parameter  | Required | Description                                       |
| ---------- | -------- | ------------------------------------------------- |
| starts_on  | true     | The date the usage starts on in format "YYYY-M-D" |
| ends_on    | true     | The date the usage ends on in format "YYYY-M-D"   |
| volume_kwh | true     | The volume in kWh during the usage period         |

### Capacity Tag Parameters

| Parameter  | Required | Description                                     |
| ---------- | -------- | ----------------------------------------------- |
| start_date | true     | The date the tag starts on in format "YYYY-M-D" |
| end_date   | true     | The date the tag ends on in format "YYYY-M-D"   |
| value_kw   | true     | The value of the tag in kw                      |

### Transmission Tag Parameters

| Parameter  | Required | Description                                     |
| ---------- | -------- | ----------------------------------------------- |
| start_date | true     | The date the tag starts on in format "YYYY-M-D" |
| end_date   | true     | The date the tag ends on in format "YYYY-M-D"   |
| value_kw   | true     | The value of the tag in kw                      |

### Response Parameters

| Parameter                    | Description                                            |
| ---------------------------- | ------------------------------------------------------ |
| id                           | The ID of the account                                  |
| status                       | The account status ("active" or "inactive")            |
| intervalization_status       | The intervalization status of the account (more below) |
| rt_lmp_price                 | The RT LMP prices the account                          |
| flow_start                   | The account's flow start date                          |
| flow_end                     | The account's flow end date                            |
| capacity_tags                | An array of the account's capacity tags                |
| transmission_tags            | An array of the account's transmission tags            |

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

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "status": "inactive",
  "intervalization_status": "intervalizing",
  "rt_lmp_price": null,
  "flow_start": null,
  "flow_end": null,
  "capacity_tags": [
    {
      "id": 1,
      "start_date": "2018-6-1",
      "end_date": "2019-5-31",
      "value_kw": 60000
    }
  ],
  "transmission_tags": [
    {
      "id": 1,
      "start_date": "2018-1-1",
      "end_date": "2018-12-31",
      "value_kw": 100000
    }
  ]
}
```

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

## Get account shadow prices

This endpoint returns the shadow prices for an individual account

```shell
curl "http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>/shadow_prices?date=<DATE>"
  -H "Authorization: Token token=MY_TRUELIGHT_TOKEN"
```

> Make sure you replace `MY_TRUELIGHT_TOKEN` with your API token

> The above command returns JSON structured like this:

```json
[
  {
    "ancillaries_price": 1.7129,
    "capacity_price": 0.1628,
    "hour": 1,
    "lmp_price": 37.0501,
    "losses_price": 2.1541248,
    "price_date": "2018-12-01",
    "rps_price": 0.261,
    "total_price_per_mwh": 41.4976248,
    "transmission_ancillaries_price": -0.0086,
    "transmission_price": 0.1653,
    "volume_kwh": 1033.31759618779
  },
  {
    "ancillaries_price": 1.7129,
    "capacity_price": 0.1628,
    "hour": 2,
    "lmp_price": 36.6938,
    "losses_price": 2.13445704,
    "price_date": "2018-12-01",
    "rps_price": 0.261,
    "total_price_per_mwh": 41.4976248,
    "transmission_ancillaries_price": -0.0086,
    "transmission_price": 0.1653,
    "volume_kwh": 1033.31759618779
  }
]
```

### HTTP Request

`GET http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>/shadow_prices<?date=DATE>`

### URL Parameters

| Parameter  | Description                              | Default   |
| ---------- | ---------------------------------------- | --------- |
| ACCOUNT_ID | The ID of the Account to retrieve        | N/A       |
| date       | The date of the shadow prices (YYYY-M-D) | Yesterday |

### Response Parameters

The response is an array of 24 objects, one for each hour of the day

| Parameter                      | Description                                     |
| ------------------------------ | ----------------------------------------------- |
| price_date                     | The date for the shadow price                   |
| hour                           | The hour of the day                             |
| ancillaries_price              | The ancillaries price for the hour              |
| capacity_price                 | The capacity price for the hour                 |
| lmp_price                      | The RT LMP price for the hour                   |
| losses_price                   | The losses price for the hour                   |
| rps_price                      | The renewable power source price for the hour   |
| total_price_per_mwh            | The sum of the other price components           |
| transmission_ancillaries_price | The transmission ancillaries price for the hour |
| transmission_price             | The transmission price for the hour             |
| volume_kwh                     | The volume in kwh for the hour                  |

<aside class="success">
A successful GET will return an HTTP 200
</aside>

<aside class="warning">
An unsuccessful GET will return an HTTP 404
</aside>
