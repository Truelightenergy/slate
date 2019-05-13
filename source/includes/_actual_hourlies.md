# Actual Hourlies

## Get the most recent actual hourlies for an account

```shell
curl -X GET \
  http://truelight.herokuapp.com/api/v1/accounts/<ACCOUNT_ID>/actual_hourlies?date=<DATE> \
  -H 'Authorization: Token token=MY_TRUELIGHT_TOKEN' \
```

> The above command returns JSON structured like this:

```json
[
  {
      "ancillaries_price": 2.0,
      "capacity_price": 1.0,
      "hour": 1,
      "lmp_price": 2.5,
      "losses_price": 2.5,
      "price_date": "2019-01-01",
      "rps_price": 1.0,
      "total_price_per_mwh": 12.5,
      "transmission_ancillaries_price": 1.5,
      "transmission_price": 2.0,
      "volume_kwh": 2.0,
  }
]
```

This endpoint retrieves actual hourly records for the given date.
If no date is passed, the most recent day of actual hourly records is returned.

### HTTP Request

`GET http://truelight.herokuapp.com/api/v1/accounts/<ACCOUNT_ID>/actual_hourlies?date=<DATE>`

### URL Parameters

| Parameter  | Description                            |
| ---------- | -------------------------------------- |
| ACCOUNT_ID | The ID of the Account to retrieve      |
| DATE       | The date ("YYYY-M-D") to be retrieved. |

### Response Parameters

A successful request will return an array of 24 objects, one for each hour of the day.

| Parameter                      | Description                                            |
| ------------------------------ | ------------------------------------------------------ |
| price_date                     | The date for the actual_hourly price                   |
| hour                           | The hour of the day                                    |
| ancillaries_price              | The ancillaries price for the hour                     |
| capacity_price                 | The capacity price for the hour                        |
| lmp_price                      | The RT LMP price for the hour                          |
| losses_price                   | The losses price for the hour                          |
| rps_price                      | The renewable power source price for the hour          |
| total_price_per_mwh            | The sum of the other price components                  |
| transmission_ancillaries_price | The transmission ancillaries price for the hour        |
| transmission_price             | The transmission price for the hour                    |
| volume_kwh                     | The volume in kwh for the hour                         |
