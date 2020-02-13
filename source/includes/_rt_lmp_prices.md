# RtLmpPrices
## Get The RtLmpPrices for that specific summary usage period

This endpoint returns an rt_lmp_prices for summary usage's date period

```shell
curl "http://truelight.herokuapp.com/api/v1/rt_lmp_prices/<SUMMARY_USAGE_ID>"
  -H "Authorization: Token token=MY_TRUELIGHT_TOKEN"
```

> Make sure you replace `MY_TRUELIGHT_TOKEN` with your API token

> The above command returns JSON structured like this:

```json
[
  {
    "id": 15316,
    "starts_on": "2019-04-10",
    "ends_on": "2019-05-09",
    "lmp_price": "24.4265",
    "ancillaries_price": "0.6452",
    "rps_price": "4.769",
    "capacity_price": "69.784",
    "losses_price": "1.7069",
    "total_price_per_mwh": "159.403",
    "transmission_price": "59.8526",
    "transmission_ancillaries_price": "-1.7812"
  }
]
```

### HTTP Request

`GET http://truelight.herokuapp.com/api/v1/rt_lmp_prices/<SUMMARY_USAGE_ID>`

### URL Parameters

| Parameter        | Description                             |
| ---------------- | --------------------------------------- |
| SUMMARY_USAGE_ID | The ID of the Summary Usage to retrieve |

### Response Parameters

| Parameter                      | Description                                            |
| ------------------------------ | ------------------------------------------------------ |
| id                             | The rt_lmp_price id                                    |
| starts_on                      | The rt_lmp_price starts_on                             |
| ends_on                        | The rt_lmp_price ends_on                               |
| lmp_price                      | The rt_lmp_price lmp_price                             |
| ancillaries_price              | The rt_lmp_price ancillaries_price                     |
| rps_price                      | The rt_lmp_price rps_price                             |
| capacity_price                 | The rt_lmp_price capacity_price                        |
| losses_price                   | The rt_lmp_price losses_price                          |
| total_price_per_mwh            | The rt_lmp_price total_price_per_mwh                   |
| transmission_price             | The rt_lmp_price transmission_price                    |
| transmission_ancillaries_price | The rt_lmp_price transmission_ancillaries_price        |
