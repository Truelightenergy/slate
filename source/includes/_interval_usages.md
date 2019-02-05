# Interval Usages

## Add interval usages to an account

```shell
curl -X POST
"http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>/interval_usages"
  --header "Authorization: Token token=MY_TRUELIGHT_TOKEN"
  --header "Content-Type: application/json" -d '{ "interval_usages": [{
"recorded_on": "2019-01-14", "hour": "1", "volume_kwh": "10.14" }, { "recorded_on":
"2019-01-14", "hour": "2", "volume_kwh": "10.14" }] }'
```

> The above command returns JSON structured like this:

```json
[
  {
    "recorded_on": "2019-01-04",
    "hour": 1,
    "id": 1,
    "volume_kwh": 10.14,
  }
  {
    "recorded_on": "2019-01-04",
    "hour": 2,
    "id": 2,
    "volume_kwh": 10.14,
  }
]
```

This endpoint creates interval usages for an account

### HTTP Request

`POST http://truelight.herokuapp.com/api/accounts/<ACCOUNT_ID>/interval_usages`

### Parameters

| Parameter                    | Required | Default | Description                                            |
| ---------------------------- | -------- | ------- | ------------------------------------------------------ |
| recorded_on                  | false    | N/A     | The date the interval usage was recorded on (YYYY-M-D) |
| hour                         | false    | N/A     | The hour of the day for the interval usage             |
| volume_kwh                   | false    | N/A     | The interval usage volume in KWH                       |
