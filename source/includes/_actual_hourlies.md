# Actual Hourlies

## Get account actual hourlies

```shell
curl "http://truelight.herokuapp.com/api/v1/accounts/<ACCOUNT_ID>/actual_hourlies?date=<DATE>"
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

This endpoint returns actual hourlies of an account

### HTTP Request

`GET http://truelight.herokuapp.com/api/v1/accounts/<ACCOUNT_ID>/actual_hourlies<?date=DATE>`

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
